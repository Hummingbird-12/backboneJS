# Backbone.js

## MVC

### **Models**

- data that can be modelled
- notify observers of changes

### **Views**

- Models' visual representation
- build and maintain DOM elements
- observe Model changes and updates

### **Controllers**

- intermediary between Models ←→ Views
- update Model when View is manipulated


## Backbone.js

### Introduction
---
- a JavaScript library, not a framework
    - library → YOU have the control
    - framework → it calls YOU
- a lot of flexibility
- dependencies
    - hard: underscore.js
    - soft: jQuery

### Base Components
---
- Backbone.Model
- Backbone.Collection
- Backbone.View
- Backbone.Router

### Internal Components
---
- Backbone.######.extend
- Backbone.Events
- Backbone.History
- Backbone.sync


### Backbone.Model
---
- contains data and logic surrounding it, such as data validation, getters & setters, default values, data initialization, conversions, etc.
- example:
    ```javascript
    var app = {}; // namespace for the app
    
    app.Todo = Backbone.Model.extend({
    	defaults: {
    		title: '',
    		completed: false
    	}
    });
    ```
- in console:
    ```javascript
    var todo = new app.Todo({
    	title: 'Say Hi',
    	completed: false
    });
    todo.get('title'); // "Say Hi"
    todo.get('created_at'); // undefined
    todo.set('created_at', Date());
    todo.get('created_at'); // the date
    ```


### Backbone.Collection
---
- ordered set of Models
    - get and set Models in the collection
    - listen for events when any element changes
    - fetch for Model's data from the server
- ~~requires reference to database, memory, etc. to save data, need to provide url parameter~~
maybe not anymore?
- example:
    ```javascript
    app.TodoList =
    	Backbone.Collection.extend({
    		model: app.Todo,
    		localStorage: new Store("todo")
    	});
    
    app.todoList = new app.TodoList();
    ```
- in console:
    ```javascript
    var todoList = new app.TodoList()
    todoList.create({ title: 'Hi' });
    
    var lmodel = new app.Todo({
    	title: 'Hello',
    	completed: true
    });
    todoList.add(lmodel);
    
    todoList.pluck('title'); // ["Hi", "Hello"]
    ```


### Backbone.View
---
- equivalent of MVC's Controllers
    - handles user events, renders HTML views, and interact with Models
- 4 basic properties
    - *el, initialize, render* and *events*
    - example:
        ```javascript
        var AppView = Backbone.View.extend({
        	// el (element) : every View has an HTML element associated to render content
        	el: '#container',
        	// initialize : first function to be called when instantiated
        	initialize: function() {
        		this.render();
        	},
        	// $el : cached jQuery object in which you can push content
        	render: function() {
        		this.$el.html("Hello World");
        	}
        });
        
        // instantiate View
        var appView = new AppView();
        ```
    - *view.el*
        - every View needs to reference a DOM at all times
            - if not specified, *this.el* is an empty *div*
    - *initialize*
        - pass parameters that will be attached to a Model, Collection or *view.el*
    - *render*
        - inject markup into elements
        - can call other View's render function
    - *events*
        - syntax:
        ```javascript
            events: { "<EVENT_TYPE> <ELEMENT_ID>": "CALLBACK_FUNCTION" };
        ```
        - example:
        ```javascript
                events: { 'keypress #new-todo': 'craeteTodoOnEnter' };
                
                (in jQuery:)
                $('#new-todo').keypress(createTodoOnEnter);
        ```
- **_.js Templates**
    - syntax:
    ```javascript
        _.template(templateString, [data], [settings]);
    ```
    → in *templateString* use the placeholder:
        - <%= %> : does not allow HTML escape
        - <%- %> : allows HTML escape
        - <% %> : run any JavaScript code
    - example:
    ```javascript
        var AppView_ = Backbone.View.extend({
          el: '#container_',
          // template which has the placeholder 'who' to be substituted later
          template: _.template("<h3>Hello <%= who %></h3>"),
          initialize: function () {
            this.render();
          },
          render: function () {
            // render the function using substituting the varible 'who' for 'World!'.
            this.$el.html(this.template({ who: 'World!' }));
          }
        });
    ```


### Backbone.Router
---
- used to reference certain 'state' or location of web application
- example:
    ```javascript
    app.Router = Backbone.Router.extend({
    	routes: {
    		'*filter': 'setFilter'
    	},
    	setFilter: function(params) {
    		console.log("params: " + params);
    		window.filter = params.trim() || '';
    	}
    });
    
    app.router = new app.Router();
    ```


### Backbone.Events
---
- can be mixed with any object and give it the pub/sub behaviour
- subscribing to Events:
    ```javascript
    object.on(event, callback, [context]);
    ```
- When event is triggered, executes the callback
- setting Events on arbitrary objects using underscore.js
    ```javascript
        var object = {},
        		callback = function(msg) { console.log("Triggered" + msg); };
        
        _.extend(object, Backbone.Events);
        
        object.on("my_event", callback);
        obejct.trigger("my_event", "my custom event");
    ```


### Backbone.#####.extend
---
- create custom classes:
    ```javascript
    var AppView = Backbone.View.extend({ ... });
    ```
- inherit:
    ```javascript
    var UserView = appView.extend({ ... });
    ```
- the constructor:
    ```javascript
    Backbone.Model.extend({
    	initialize: function() {
    		console.log("Model created!");
    	}
    });
    ```
