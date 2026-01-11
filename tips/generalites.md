
## IC : Inversion of Control
- Dependency injection
- Cf [Baeldung article](https://www.baeldung.com/constructor-injection-in-spring)

  Strong coupling : Static instanciation   
  Class A --> Interface B --> Class BImpl
  ```java
  class A {
    B b = new BImpl();
  }
  ```
  
  Loose coupling : Dynamic instanciation    
  ```java
  @Component
  class A {
  
    @Autowired
    B b;
  }
  
  interface B {
  ...
  }
  
  @Component
  class BImpl implements B  {
  ...
  }
  
  ```
  

## AOP : Aspect Oriented Programming  
Paradigme au coeur de Spring  
- Principe de séparation de préoccupations (concerns)  
- Aspect : fonctionnalité transverse (sécurité, logging, tracing, persistance, tolérance aux fautes, synchronisation, ...)
- Notion de tissage d'aspects (aspect weaver)
- "joint point" (point de jonction) : point de jonction dans le programme
- "advice" (méthode d'aspect) : traitement additionnel 
- "pointcut" (coupe transversale) : ensemble logique de points de jonctions 
