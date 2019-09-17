---
layout: post
title: "Anatomy of a JavaScript MV* Framework"
categories: JavaScript MV
published: true
---

The main features of an MV* application are routing, data binding, templates/views, models, and data access.

# #Web Application Architectures
- **Client-Side Web Applications**
    - You get Single Page (shell) and then multiple AJAX request follows to fill up <div> placeholders with HTML Templates and to get Data from Web API. 
        - Angular JS
        - Backbone
        - Ember
        - Knockout
- **Server-Side Web Applications**
    - You get HTML Templates + Data from Web Server
        - Ruby: Rails, Sinatra
        - .NET: ASP, ASP.NET, ASP.NET MVC
        - Java: JSP, Spring
        - PHP: Cake, Codelgniter, Larvel

- **MVC/MV* Frameworks:**
    - Templates
    - Router
    - Controller
    - Data binding
    - Models
    - Web Components
    - Data Access

# #Template/Views
## #Angular
- **templates dir:**
    - templates/home.html
        - `<h1>Home</h1>`
    - templates/about.html
        - `<h1>About</h1>`
- **script tag:**
    - /index.html

    ```html
    <script type="text/ng-template" id="home">
        <h1>Home</h1>
    </script>
        
    <script type="text/ng-template" id="about">
        <h1>About</h1>
    </script>
    ```

## #ember
- **templates dir:**
    - templates/home.html
        - `<h1>Home</h1>`
    - templates/about.html
        - `<h1>About</h1>`
- **script tag:**
    - /index.html

    ```html
    <script type="text/x-hanlebars" data-template-name="home">
        <h1>Home</h1>
    </script>
        
    <script type="text/x-hanlebars" data-template-name="about">
        <h1>About</h1>
    </script>
    ```

## #Backbone.js
- **templates dir:**
    - templates/home.html
        - `<h1>Home</h1>`
    - templates/about.html
        - `<h1>About</h1>`
- **script tag:**
    - /index.html

    ```html
    <script type="text/template" id="home">
        <h1>Home</h1>
    </script>
        
    <script type="text/template" id="about">
        <h1>About</h1>
    </script>
    ```

# #Router
## #Angular

```js
angular.module('FunnyAnt.Examples.Routing', ['ngRoute'])
.config(function ($routeProvider) {
    $routeProvider

    .when('/home', {
        templateUrl: 'templates/home.html'
    })

    .when('/about', {
        templateUrl: 'templates/about.html'
    })

    .otherwise({
        redirectTo: '/home'
    });
})
```

## #ember

```js
App = Ember.Application.create();

App.Router.map(function() {
    //first parameter refers to the template
    this.route('home', { path: '/' });
    this.route('about');
});
```

## #Backbone.js

```js
var Approuter = Backbone.Router.extends({
    routes: {
        '': 'homeRoute',
        'home': 'homeRoute',
        'about': 'aboutRoute'
    },
    homeRoute: function () {
        var homeView = new HomeView();
        $("#content").html(homeView.el);
    },
    aboutRoute: function() {
        var aboutView = new AboutView();
        $("#content").html(aboutView.el);
    }
});

var appRouter = new AppRouter();
Backbone.history.start();
```

# #Controllers
- **Two Jobs:**
    - Get data from API and make it avaiable to view.
    - Handle user intrection.

## #Angular

```js
angular.module('FunnyAnt.Examples.Controllers')
.controller('AboutController', function($scope) {

    $scope.page = {
        bodyText: 'This is the about page.',
        detailsVisible: false
    };

    $scope.toggleDetails = function() {
        $scope.page.detailsVisible = !$scope.page.detailsVisible;
    }
});
```

```html
<button ng-click="toggleDetails()">Detail</button>
<p ng-show="page.detailsVisible">
{{page.bodyText}}
</p>
```

## #ember

```js
// Route object coordinating controller.
// Load the model
App.AboutRoute = Ember.Route.extend({
    model: function() {
        return {
            page: {
                bodyText: 'This is the abuot page.'
            }
        }
    }
});

// Mediating controller.
// Handel user intraction
App.AboutController = Ember.ObjectController.extends({
    detailsVisible: false,
    actions: {
        toggleDetails: function() {
            this.set('deatilsVisible', !this.get('detailsVisible'));
        }
    }
});
```


```ember
<button {action='toggleDetails'}>Details</button>
{#if detailsVisible}
    <p>{page.bodyText}</p>
{/if}
```

## #Backbone

```js
var AboutView = Backbone.View.extend({
    template: _.template($("about-template").html()),
    events: {
        "click .detail": "toggleDetails"
    },
    initialize: function () {
        this.render();
    },
    render: function () {
        this.$el.html(this.template({ page: {
                bodyText: "This is the about page."
            } 
        }));
        this.$details = this.$("details");
    },
    toggleDetails: function() {
        this.$details.toggle();
    }
});
```

```html
<button class="details">Detail</button>
<p id="details" style="display:none;">
    <%= page.bodyText %>
</p>
```

# #Data Binding
## #Angular

```html
Name: <input type="text" ng-model="name" />
<br />
Welcome to AngularJS {name}
```

## #ember

```ember
Name: {input type="text" value=name}
<br />
Welcome to Ember {name}
```

## #Backbone.js
- Do not have two-way data  by default.
- Use plugin

```html
<div id="message-container">
    <input id="name"/>
    <br />
    <span id="message"></span>
</div>

<script type="text/template" id="message-template">
    Welcome to Backbone <%=name%>
</script>
```

```js
var MessageView = Backbone.View.extends({
    template: _.template($('message-template').html()),
    events: {
        'keyup #name': 'updateModel'
    },
    updateModel: function(event) {
        this.model.set({ name: $("name").val() });
        this.render();
    },
    render: function() {
        this.$("#message").html(this.template(this.model.toJSON()));
        return this;
    }
});

var person = new Backbone.Model({ name: '' });
messageView = new MessageView({
    el: $('3message-contianer'), model: person
})
```

# #Models
- Data for application.

## #Angular

```js
angular.module('demo', [])
.service('Person', function() {
    this.name = "Craig McKeachie";
})

.controller('DemoController', function($scope, Person) {
    $scope.person = Person;

    $scope.changeName = function() {
        $scope.person.name = "John Doe"
    }

    $scope.$watch("person.name", function(newValue, oldValue) {
        $scope.oldValue = oldValue;
        $scope.newValue = newValue;
    });
})
```

## #ember

```js
App.Person = Ember.Object.extend({
    name: 'Craig McKeachie',
    nameChanged: function() {
        console.log("name changed.");
    }.observes('name').on('init')
});

App.IndexController = Ember.ObjectController.extend({
    actions: {
        changeName: function() {
            this.get('person').set('name', "John Doe");
        }
    }
});
```

## #Backbone.js

```js
var Person = Backbone.Model.extend({
    defaults: {
        name: 'Craig McKeachie',
        perviousName: ''
    },
    initialize: function() {
        this.on('change:name', this.nameChanged);
    },
    nameChanged: function(options) {
        console.log(options.previous('name'));
        this.set('previousName', options.previous('name'));
    }
});

var IndexView = Backbone.View.extend({
    template: _.template($("#index-template").html())
    events: {
        "click .changeName": "changeName"
    },
    // ...
    changeName: function() {
        this.model.set('name', 'John Doe');
        this.render();
    }
});
```

# #Web Components
## #Angular

```js
angular.module('demo', [])
.directive('prefixHello', function() {
    return {
        template: '<div>Hi there.</div>'
    };
})
```

```html
<prefix-hello></prefix-hello>
```

## #ember

```ember
{prefix-hello}


<script type="text/x-handlebars" data-template-name="components/prefix-hello">
    <div>Hi there.</div>
</script>
```

## #Backbone.js

```js
var IndexView = Backbone.View.extend({
    template: _.template($("#index-template").html()),
    //...
    render: function() {
        this.$el.html(this.template());
        var helloView = new HelloView();
        this.$('[prefix-hello]').html(helloView.el);
    }
});

var HelloView = Backbone.View.extend({
    temlpate: _.template($("#hello-template").html()),
    initialize: function() {
        this.render();
    },
    render: function() {
        this.$el.html(this.template());
    }
});
```

```html
<div prefix-hello>
</div>

<script type="text/template" id="hello-template">
    Hi there.
</script>
```

# #Data Access
## #Angular

```js
$http({
    method: 'get',
    url: 'data/employees.json'
})
.then(function(response) {
    $scope.employees = response.data;
});
```

## #ember

```js
App.EmployeesRoute = Ember.Route.extend({
    model: function() {
        return $.ajax('data/employees.json');
    }
});
```

## #Backbone.js

```js
var app = app || {};

app.Employee = Backbone.Model.extend({
    default: {
        firstName: '',
        lastName: ''
    }
});

app.Employees = Backbone.Collection.extend({
    model: app.Employee,
    url: 'data/employees.json'
});

app.employees = new app.Employees();
app.employees.fetch({
    reset: true
});
```