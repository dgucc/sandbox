Très bonne question ! 🙌  
**Material Design** (de Google) est souvent associé à **Angular Material**, mais il existe des façons de l’utiliser **sans Angular** — même sans framework JavaScript.

Voici **comment tester Material Design (avec `mat-*` composants comme `mat-slide-toggle`, `mat-button-toggle`, etc.) sans Angular**, en utilisant uniquement :

- ✅ **HTML**
- ✅ **CSS**
- ✅ **JavaScript pur**
- ✅ **Librairies CSS Material Design indépendantes**

---

## ✅ Solution : Utiliser **Material Design for Bootstrap (MDB)** ou **Material Components Web (MDC-Web)**

Deux options principales existent pour utiliser des composants Material **sans Angular** :

| Option | Description | Recommandé pour ? |
|--------|-------------|-------------------|
| 🔹 **[Material Components Web (MDC-Web)](https://material.io/components/web)** | Version officielle de Google, pure JS/CSS, **pas dépendante d’un framework** | ✅ **Recommandé si vous voulez le vrai Material Design** |
| 🔹 **[MDB (Material Design for Bootstrap)](https://mdbootstrap.com/)** | Version "Material-style" sur Bootstrap 5 — plus simple à intégrer | ✅ **Recommandé si vous connaissez déjà Bootstrap** |

---

## 🎯 Méthode recommandée : **Material Components Web (MDC-Web)** → *Sans Angular, sans React, sans Vue*

C’est la **version officielle de Google**, conçue pour être **framework-agnostic**. Vous pouvez l’utiliser avec du HTML pur + JS vanilla.

---

### ✅ Étape 1 : Intégrer MDC-Web dans votre projet

Utilisez les CDN (pas besoin de Node.js ou npm) :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Test Material Design sans Angular</title>

  <!-- MDC Web CSS -->
  <link href="https://unpkg.com/material-components-web@latest/dist/material-components-web.min.css" rel="stylesheet">

  <!-- Google Fonts (Roboto) -->
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Roboto', sans-serif;
      padding: 2rem;
      background-color: #f5f5f5;
    }

    .toggle-group {
      display: flex;
      flex-direction: column;
      gap: 16px;
      max-width: 300px;
    }
  </style>
</head>
<body>

  <h2>Toggle Vertical avec Material Design (sans Angular)</h2>

  <!-- Groupe de boutons toggle verticaux (simulé avec mat-button-toggle) -->
  <div class="toggle-group">
    <div class="mdc-form-field">
      <div class="mdc-switch">
        <input type="checkbox" class="mdc-switch__native-control" id="toggle1" value="option1">
        <div class="mdc-switch__track"></div>
        <div class="mdc-switch__thumb-underlay">
          <div class="mdc-switch__thumb"></div>
        </div>
      </div>
      <label for="toggle1">Option 1</label>
    </div>

    <div class="mdc-form-field">
      <div class="mdc-switch">
        <input type="checkbox" class="mdc-switch__native-control" id="toggle2" value="option2">
        <div class="mdc-switch__track"></div>
        <div class="mdc-switch__thumb-underlay">
          <div class="mdc-switch__thumb"></div>
        </div>
      </div>
      <label for="toggle2">Option 2</label>
    </div>

    <div class="mdc-form-field">
      <div class="mdc-switch">
        <input type="checkbox" class="mdc-switch__native-control" id="toggle3" value="option3">
        <div class="mdc-switch__track"></div>
        <div class="mdc-switch__thumb-underlay">
          <div class="mdc-switch__thumb"></div>
        </div>
      </div>
      <label for="toggle3">Option 3</label>
    </div>
  </div>

  <div class="mt-4 p-3 bg-light rounded" style="margin-top: 2rem;">
    <strong>Option sélectionnée :</strong> 
    <span id="selected-value">Aucune</span>
  </div>

  <!-- MDC Web JS -->
  <script src="https://unpkg.com/material-components-web@latest/dist/material-components-web.min.js"></script>

  <script>
    // On va gérer manuellement la logique "une seule option activée"
    const switches = document.querySelectorAll('.mdc-switch');
    const selectedValue = document.getElementById('selected-value');

    // Initialisation des switches MDC
    switches.forEach(switchEl => {
      new mdc.switchToggle.MDCSwitchToggle(switchEl);
    });

    // Logique : une seule option peut être active
    switches.forEach((switchEl, index) => {
      const input = switchEl.querySelector('input');
      input.addEventListener('change', () => {
        if (input.checked) {
          // Désactiver tous les autres
          switches.forEach(otherSwitch => {
            const otherInput = otherSwitch.querySelector('input');
            if (otherInput !== input) {
              otherInput.checked = false;
              otherSwitch.classList.remove('mdc-switch--checked'); // Force réendre
            }
          });
          // Mettre à jour l'affichage
          selectedValue.textContent = input.value;
        }
      });
    });
  </script>

</body>
</html>
```

---

## ✅ Résultat obtenu

Vous avez maintenant un **groupe vertical de 3 `mat-slide-toggle`** avec :

- ✅ Le **design exact de Material Design** (Google officiel)
- ✅ Animation fluide et transition
- ✅ Style tactile et responsive
- ✅ **Pas d’Angular, pas de React, pas de framework**
- ✅ Fonctionne dans n’importe quel navigateur moderne

> 💡 Les `mdc-switch` sont l’équivalent direct de `mat-slide-toggle`.  
> Pour un équivalent de `mat-button-toggle` (bouton carré), utilisez `mdc-button` avec `--outlined` et gérer la sélection manuellement.

---

## 🔧 Bonus : Équivalent de `mat-button-toggle-group` (boutons carrés)

Si vous préférez des boutons carrés (comme `mat-button-toggle`) au lieu de sliders :

```html
<div class="toggle-group">
  <button class="mdc-button mdc-button--outlined" data-value="option1">
    <span class="mdc-button__label">Option 1</span>
  </button>
  <button class="mdc-button mdc-button--outlined" data-value="option2">
    <span class="mdc-button__label">Option 2</span>
  </button>
  <button class="mdc-button mdc-button--outlined" data-value="option3">
    <span class="mdc-button__label">Option 3</span>
  </button>
</div>
```

Et le JS :

```js
const buttons = document.querySelectorAll('.mdc-button--outlined');
buttons.forEach(btn => {
  btn.addEventListener('click', () => {
    buttons.forEach(b => b.classList.remove('mdc-button--activated'));
    btn.classList.add('mdc-button--activated');
    selectedValue.textContent = btn.dataset.value;
  });
});
```

Ajoutez ce CSS pour le style actif :

```css
.mdc-button--activated {
  background-color: #e3f2fd !important;
  color: #1976d2 !important;
  border-color: #1976d2 !important;
}
```

---

## ✅ Avantages de cette approche

| Avantage | Explication |
|---------|-------------|
| ✅ **Officiel** | Google fournit MDC-Web — c’est la source authentique |
| ✅ **Framework-free** | Pas besoin d’Angular, React, Vue |
| ✅ **Modulaire** | Vous chargez seulement ce dont vous avez besoin |
| ✅ **Accessible** | Respecte WCAG, supporte clavier, lecteurs d’écran |
| ✅ **Production-ready** | Utilisé par Google dans ses propres produits |

---

## 🚫 Ce que vous évitez

- ❌ Pas besoin d’installer Node.js
- ❌ Pas besoin de Webpack / Vite / Angular CLI
- ❌ Pas besoin de TypeScript
- ❌ Pas de build complexe

---

## ✅ Conclusion : Comment tester Material sans Angular ?

> ✅ **Réponse courte** :  
> Utilisez **Material Components Web (MDC-Web)** via CDN (`<link>` et `<script>`), puis initialisez les composants avec du JavaScript pur. C’est **officiel, léger et sans framework**.

> ✅ **Exemple rapide** : Copiez-collez le premier code complet ci-dessus dans un fichier `.html`, ouvrez-le dans Chrome → vous avez un vrai `mat-slide-toggle` vertical sans Angular !

---

💡 **Astuce bonus** :  
Visitez [https://material-components.github.io/material-components-web-catalog/#/component/switch](https://material-components.github.io/material-components-web-catalog/#/component/switch) pour explorer les composants en live — et copier leur code HTML/CSS/JS directement.

---

Vous venez de tester **Material Design officiel**… sans Angular. Félicitations ! 🎉
