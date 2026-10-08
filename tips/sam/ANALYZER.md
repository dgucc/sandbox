# Fonctionnement de `DependencyAnalyzer`

`DependencyAnalyzer` transforme un ensemble de fichiers Java en un graphe de dépendances entre fichiers. Il s'appuie sur l'API d'arbres du compilateur du JDK pour analyser la syntaxe Java, mais ne compile pas le projet étudié.

## Résultat produit

La méthode publique est :

```java
DependencyGraph analyze(Path sourceRoot) throws IOException
```

Chaque nœud du graphe correspond à un fichier source, identifié par son package déclaré et son nom de fichier sans l'extension `.java` (par exemple `com.example.Client`). Une arête `A -> B` signifie que le fichier `A` dépend d'au moins un type déclaré dans le fichier `B`.

Les dépendances sont limitées aux types trouvés dans le répertoire analysé. Les types de la bibliothèque standard, des dépendances externes et des autres emplacements absents de ce répertoire ne deviennent pas des nœuds du graphe.

## Étapes d'analyse

### 1. Vérifier et parcourir le répertoire

La méthode normalise le chemin en chemin absolu et vérifie que celui-ci désigne un répertoire. Sinon, elle lève une `IllegalArgumentException`.

Elle parcourt ensuite récursivement le répertoire avec `Files.walk`, conserve les fichiers réguliers dont le nom se termine par `.java`, puis les trie. Le tri rend l'ordre de traitement stable.

### 2. Analyser la syntaxe avec le compilateur du JDK

L'analyseur récupère le compilateur système avec `ToolProvider.getSystemJavaCompiler()`. Si celui-ci est absent — cas courant lorsque seule une JRE est installée — la méthode lève une `IOException` indiquant qu'un JDK est requis.

Les fichiers sont fournis à une tâche `JavacTask` configurée avec l'option `-proc:none`, qui désactive le traitement des annotations. L'appel à `task.parse()` construit des arbres syntaxiques (`CompilationUnitTree`) sans effectuer la compilation ni la résolution sémantique complète du projet.

Un `DiagnosticCollector` recueille les diagnostics du parseur. Si au moins un diagnostic est de type `ERROR`, l'analyse s'arrête et lève une `IOException` comprenant le fichier, le numéro de ligne et le message de chaque erreur. Les erreurs de résolution sémantique ne sont pas recherchées comme lors d'une compilation, puisque l'analyseur ne lance pas `task.analyze()`.

### 3. Extraire les informations de chaque fichier

Pour chaque unité de compilation, `readUnit` crée une représentation interne `SourceUnit` contenant :

- `nodeName` : nom du nœud du graphe, formé du package déclaré et du nom de fichier sans `.java` ;
- `packageName` : package déclaré, ou chaîne vide pour le package par défaut ;
- `types` : types déclarés dans le fichier ;
- `imports` : imports explicites et imports avec joker ;
- `referencedNames` : noms simples rencontrés dans les arbres `IdentifierTree` ;
- `qualifiedReferences` : noms qualifiés rencontrés dans les arbres `MemberSelectTree`.

Un `TreePathScanner` visite les déclarations de types. Il conserve une pile logique des types englobants afin de former les noms qualifiés des types imbriqués, par exemple `com.example.Outer.Inner`. Chaque déclaration est représentée par un `SourceType` qui mémorise son nom qualifié, son nom simple et son package.

Un second passage sur les imports les convertit en `ImportReference(name, wildcard)`. Les imports statiques sont ramenés au type qui les porte : `import static com.example.Utility.run;` est ramené à `com.example.Utility`, tout comme `import static com.example.Utility.*;`. Un import ordinaire avec joker, tel que `import com.example.helpers.*;`, conserve le nom du package et le marque comme wildcard.

Enfin, deux autres scanners collectent les identifiants et les sélections qualifiées présents dans l'arbre syntaxique. Ces collections sont des ensembles triés, ce qui élimine les répétitions au sein d'un fichier et stabilise leur parcours.

### 4. Construire l'index des types locaux

`buildGraph` commence par ajouter un nœud pour chaque `SourceUnit`, y compris les fichiers sans dépendance détectée. En parallèle, il construit deux index :

- `typesByQualifiedName` associe un nom qualifié à l'unité qui contient le type ;
- `typesBySimpleName` associe un nom simple à la liste des types locaux portant ce nom.

Ces index permettent de comparer les noms trouvés dans le code avec les déclarations présentes sous la racine analysée. Un import ou une référence sans correspondance locale n'ajoute donc aucune arête.

### 5. Résoudre les dépendances de chaque fichier

Pour chaque unité source, l'analyseur examine trois sortes d'indices :

1. **Imports explicites** — le nom qualifié est recherché dans `typesByQualifiedName`. S'il correspond à un type local, le fichier qui contient ce type devient une dépendance.
2. **Noms simples** — chaque identifiant est comparé aux types locaux de même nom simple. Une correspondance est retenue si le type est dans le même package ou dans un package couvert par un import wildcard de l'unité.
3. **Références qualifiées** — chaque sélection qualifiée est comparée aux noms qualifiés des types locaux. Une correspondance est retenue si elle est exactement égale au nom du type ou commence par ce nom suivi d'un point. Cela permet notamment de reconnaître l'accès à un membre d'un type, comme `com.example.Service.run`.

Les indices peuvent retrouver plusieurs fois la même dépendance. `DependencyGraph` stocke les cibles dans des ensembles triés : les arêtes dupliquées sont ainsi éliminées et le résultat reste ordonné.

Avant d'ajouter une arête, `addDependency` ignore les cibles absentes et les dépendances vers le même nœud source. Ce choix modélise les dépendances **entre fichiers** : une référence à un autre type défini dans le même fichier ne crée pas de boucle dans le graphe.

## Modèle des types et des fichiers

Le graphe est construit au niveau du fichier, et non au niveau de chaque classe. Un fichier qui contient plusieurs classes reste donc un seul nœud, même si toutes ses déclarations de types sont enregistrées dans les index pour résoudre les références.

Le nom de nœud utilise le nom du fichier et le package déclaré dans le code. Il ne dépend pas du chemin relatif réel des répertoires. Le projet suppose que les noms de fichiers et packages permettent d'identifier les unités sans collision.

## Limites de la résolution

La résolution est une analyse syntaxique fondée sur des noms, pas une résolution de symboles par le compilateur. En conséquence :

- seuls les fichiers sous la racine fournie sont considérés ; le classpath et les dépendances externes ne sont pas parcourus ;
- les imports wildcard sont interprétés à partir du package et des types présents dans ce répertoire ;
- les références par nom simple peuvent être ambiguës ; l'analyseur ne consulte pas la sémantique du compilateur pour choisir entre plusieurs types homonymes ;
- comme les identifiants et sélections sont recueillis dans l'arbre syntaxique, un nom rencontré n'est pas toujours une référence effective à un type ; certaines situations complexes peuvent donc produire des dépendances manquantes ou supplémentaires ;
- les dépendances à l'intérieur d'un même fichier sont volontairement ignorées dans le graphe.

Ces choix rendent l'outil léger et permettent d'analyser un projet sans construire ses dépendances. Le résultat est une vue pratique des relations locales entre fichiers, plutôt qu'une preuve complète de dépendance sémantique.

## Exemple simplifié

Avec les fichiers suivants :

```java
// app/Client.java
package app;
import lib.Service;
class Client { Service service; }
```

```java
// lib/Service.java
package lib;
class Service { }
```

si les deux fichiers se trouvent sous la racine analysée, le graphe contient les nœuds `app.Client` et `lib.Service`, ainsi que l'arête :

```text
app.Client -> lib.Service
```

La détection des cycles est ensuite effectuée par `DependencyGraph`, à l'aide de l'algorithme de Tarjan sur le graphe ainsi construit.
