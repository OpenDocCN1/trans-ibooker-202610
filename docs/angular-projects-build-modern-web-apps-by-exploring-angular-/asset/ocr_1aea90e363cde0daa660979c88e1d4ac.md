An Angular application typically has one main component, called **AppComponent**, by convention. Each component in the tree can communicate and interact with its siblings using an application programming interface defined by each one.

An Angular application can have many features that are called modules. Each module serves a block of single functionality that corresponds to a particular application domain or workflow. Angular modules are used to group Angular components that share similar functionality:

![](bbox=[0.333, 0.272, 0.657, 0.543])

In the previous diagram, the dashed line circles represent Angular modules. An Angular application typically has one main module, called **AppModule**, by convention. Each module can import other modules in an Angular application if they wish to use part of their functionality.

The functionality of a module can be further analyzed in the presentational and business logic of a feature. Angular components should only be responsible for handling the presentational logic and delegating business logic tasks to services. The Angular framework provides Angular services to components using a built-in **dependency injection (DI)** mechanism.