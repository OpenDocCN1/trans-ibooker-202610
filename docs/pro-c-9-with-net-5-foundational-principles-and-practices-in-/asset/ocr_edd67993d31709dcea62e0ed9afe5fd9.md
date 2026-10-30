This convention of dropping the suffix is repeated through ASP.NET Core. There will be many examples covered in the following chapters.

Another naming convention is used in view location and selection. By default, an action method (in an MVC application) will render the view of the same name as the method. Editor and display templates are named after the class that they render in the view. These defaults can be changed if your application requires it. All of these will be further explored when the AutoLot.Mvc application is built.

# Directory Structure

There are several folder conventions that you must understand to successfully build ASP.NET Core web applications and services.

# The Controllers Folder

By convention, the Controllers folder is where the ASP.NET Core MVC and API implementations (and the routing engine) expect that the controllers for your application are placed.

# The Views Folder

The Views folder is where the views for the application are stored. Each controller gets its own folder under the main Views folder named after the controller name (minus the Controller suffix). The action methods will render views in their controller’s folder by default. For example, the Views/Home folder holds all the views for the HomeController controller class.

# The Shared Folder

A special folder under Views is named Shared. This folder is accessible to all controllers and their action methods. After searching the folder named for the controller, if the view can’t be found, then the Shared folder is searched for the view.

# The wwwroot Folder (New in ASP.NET Core)

An improvement over ASP.NET MVC is the creation of a special folder named wwwroot for ASP.NET Core web applications. In ASP.NET MVC, the JavaScript files, images, CSS, and other client-side content were intermingled with all the other folders. In ASP.NET Core, the client side is all contained under the wwwroot folder. This separation of compiled files from client-side files significantly cleans up the project structure when working with ASP.NET Core.

# Controllers and Actions

Just like ASP.NET MVC and ASP.NET Web API, controllers and action methods are the workhorses of an ASP.NET Core MVC or API application.

# The Controller Class

As mentioned already, ASP.NET Core unified ASP.NET MVC5 and ASP.NET Web API. This unification also combines the Controller, ApiController, and AsyncController base classes from MVC5 and Web API 2.2 into one new class, Controller, which has a base class of its own named ControllerBase. ASP.NET