# pipeline-routing

A simple router to be used with [Pipeline](https://github.com/rimunroe/pipeline).

## Defining routes

A route is defined through an object with the following structure:

```javascript
{
  name: 'home',
  path: '/',
  dynamic: false,
  pattern: /[a-zA-Z0-9\+\(\)\-\_]+/
  handler: function(page, params){/*...*/},
  children: [/*...*/]
}
```

### name

The name is a string, and must be unique among all route names.

### path

This is the route's path, relative to the parent.

*Note*: The only path where a forward slash can appear is the root.

### pattern (optional)

A regular expression used to test whether or not a URL matches the current route.

### dynamic (optional)

A boolean indicating whether or not this path represents a dynamic segment. If it is true, then anything matching its pattern will be used as a parameter.

### handler (optional)

This is the function which will be called when the route is hit. It takes two arguments: the new page's name and an object listing the keys and values of any dynamic route segments at the current location.

The default handler for all routes is to fire the navigation action.

### children (optional)

This is an array containing route definition objects with the same structure.

## Example use

```javascript
var pipeline = require('pipeline');
var routing = require('pipeline-routing');

var app = pipeline.createApp({
  initialize: function(){
    console.log('app initialization hook');
  },
  plugins: [
    routing
  ]
});

app.create.router({
  routes: {
    name: 'home',
    path: '/',
    children: [{
      name: 'list',
      path: 'list',
      children: [{
        name: 'item',
        path: ':itemId',
        dynamic: true
      }]
    }]
  }
});

app.start();

```

## Defaults

Creating a router is done by invoking `app.create.router(options)`. `options` is an object with the following structure:

```javascript
{
  routes: {/*...*/},

  defaults: {
    action: {
      name: 'navigate',
      validator: function(page, params){}
    },

    store: {
      name: 'location',
      actions: {
        navigate: function(page, params){}
    },

    adapter: {
      name: 'router',
      stores: {
        location: function(){}
      }
    }
  }
}
```

All defaults are optional.

## Installation

`npm install pipeline-routing`
