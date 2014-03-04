#Angular Best Practices

##Feature Driven Development
See: http://www.johnpapa.net/angular-growth-structure/

Feature driven development is more about how you organise and structure your application than how you code the functionality. Rather than produce 1 large complicated app you create discreet pieces and couple them together. 

###By type - No
Files organised by type means having your controllers, services, filters, directives, views etc. placed in their respective folders. Nothing is discrete, development and testing is done in large blocks. Large applications quickly become overly complex.

    |
    |- controllers
    |--- controllers.mod.js
    |--- home.ctrl.js
    |--- about.ctrl.js
    |--- contact.ctrl.js
    |--- login.ctrl.js
    |- services
    |--- auth.svc.js

###By Feature - Yes
The application is organised by the self-contained features that it requires. Each feature can be plugged into or out of the application with relative ease. It is not baked directly into the application’s foundations. Both development and testing is conducted within the scope of each feature.

    |
    |- home
    |--- home.mod.js
    |--- home.ctrl.js
    |- about
    |--- about.mod.js
    |--- about.ctrl.js
    |- contact
    |--- contact.mod.js
    |--- contact.ctrl.js
    |- login
    |--- login.mod.js
    |--- login.ctrl.js
    |--- auth.svc.js

##Dependency Injection
See: http://docs.angularjs.org/guide/dev_guide.services.managing_dependencies

There are two types of dependencies you can include in your modules and controllers. The first is listing your required modules and the second is injecting services into your controllers.

###Requiring modules

    angular.module('dashboard', [
    	'dashboard.common.directives',
    	'dashboard.features.home',
    	'dashboard.features.about',
    	'dashboard.features.contact',
    	'dashboard.features.login'
    ])

This is a fairly standard process of how we require modules in AngularJS. Naming conventions are covered in the next section but you can see here that required modules should be grouped by feature.

###Injecting dependencies

    .controller('homeCtrl', ['$scope', '$state', function ($scope, $state) { }])

To help when minifying code and keeping your dependencies under control.

##Naming Conventions
It is important to provide distinct names to components of your application. This mitigates the possibility that the angular source, 3rd party libraries and shared components will have clashing module names, giving unexpected results.

###Scoping components
All components (directives, controllers, services etc.) should be prefixed with an identifier that is relevant to their scope. E.g. 'dashboard.' for an app called "dashboard".

###Element list and patterns to follow
To aid readability in the solution the type of component should be appended to each file name, for example:
- dashboard.mod.js  - the module declaration
- dashboard.dir.js – the directive
- dashboard.svc.js – the service or factory
- dashboard.ctrl.js – the controller
- dashboard.flt.js – any filters 
- dashboard.tpl.html – the template
- dashboard.spec.js– the unit tests

##HTML Templates and Logic
See: http://docs.angularjs.org/guide/templates

"An Angular template is the declarative specification that, along with information from the model and controller, becomes the rendered view that a user sees in the browser. It is the static DOM, containing HTML, CSS, and angular-specific elements and angular-specific element attributes. The Angular elements and attributes direct angular to add behaviour and transform the template DOM into the dynamic view DOM."

##Rules
Don't write HTML in your JavaScript *(unless you are building a distributable directive where templateUrl won’t function correctly, or it’s an abstract state, see routing section)*.

    $scope.errorMsg = "<strong>Validation failed</strong>";
    
Don’t use logic in your HTML templates, for example the following should not be done:

    <div class="alert" ng-show="players > 20">Too many players</div>

You should declare a property or method on scope and use that to drive your HTML conditions.

Using a declarative approach to logic provides a more maintainable template. If we used `players > 20` on multiple HTML elements it would create additional overhead if we were to update that condition. By encapsulating the logic in a property or method it provides both semantically correct mark-up and re-usability.

##Routers
See: https://github.com/angular-ui/ui-router

###AngularUI Router
All routing should be conducted using AngularUI Router. AngularUI Router is an external plugin used to convert your application into a state machine. ngRoute uses the URL to drive navigation. uiRouter reverses your app's relationship with the URL. It uses states to navigate and it drives changes to the URL rather than the URL driving changes to your app. [Please familiarise yourself with the AngularUI Router documentation](https://github.com/angular-ui/ui-router)
