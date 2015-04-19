<!-- .slide: data-background-color="rgb(0, 75, 80)" data-background-image="assets/microsoft.svg" data-background-position="3.5% 93.5%" data-background-size="256px" -->

# React - What's the big deal?
## Daniel Perez Alvarez
## @unindented

Note:

This presentation tries to explain why I think React is a really important milestone for frontend development.



## What makes a UI difficult to build?


<!-- .slide: data-background-image="assets/change-base.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

What we are doing with a UI is mapping a bunch of data structures to something visible on the screen. That's pretty easy though.


<!-- .slide: data-background-image="assets/change-change.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

Things get challenging when those data structures change over time, as the user interacts with the app, the server provides new data, etc.


- Lots of state
- Difficult to test
- Limited libraries

Note:

- In a UI there's lots of state, which means lots of complexity.
- You can automate some tests, but things almost always require human verification.
- Our libraries are pretty limited.



<!-- .slide: data-background="rgb(0, 75, 80)" -->

## Server-Side Rendering


<!-- .slide: data-background-image="assets/change-reload.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

Pages are immutable.

Pros:

- Every interaction means a page reload, so the frontend has very little state to manage.

Cons:

- Re-rendering the whole UI on every change makes for a bad UX.
- It also puts extra load on the backend.



<!-- .slide: data-background="rgb(0, 75, 80)" -->

## Manual Rendering


<!-- .slide: data-background-image="assets/change-manual.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

The UI notifies us that the user interacted with the UI, but we have to do all the work.


### The DOM

Note:

The Document Object Model (DOM) is a programming interface for HTML and XML documents. It provides an object-oriented representation of the document, and it can be modified with a scripting language such as JavaScript.


<!-- .slide: data-type="codes one-column" -->
```javascript
window.onload = function () {
  var doc = window.document;
  var el = doc.createElement('h1');
  var text = doc.createTextNode('Hi!');
  el.style.color = 'red';
  el.appendChild(text);
  doc.body.appendChild(el);
};
```


<iframe style="height: 480px; width: 480px;" scrolling="no" src="//codepen.io/unindented/embed/ZbYzOK/?height=480"></iframe>

Note:

We have to take care of everything.

Pros:

- Doesn't re-render the whole UI on every interaction.

Cons:

- Complex and full of quirks.
- Doesn't enforce any sort of separation of concerns.
- Provides no means of composition.


### jQuery

Note:

jQuery is a JavaScript library designed to make it easier to navigate a document, select DOM elements, create animations, handle events, and fire Ajax requests.

Originally released in 2006, it is in use on over 63% of the top million most popular sites by traffic volume.


<!-- .slide: data-type="codes one-column" -->
```javascript
$(function () {
  var heading = $('<h1/>', {
    text: 'Hello world!',
    css: {color: 'red'}
  }).appendTo('body');
});
```


<iframe style="height: 480px; width: 480px;" scrolling="no" src="//codepen.io/unindented/embed/zvxOGd/?height=480"></iframe>

Note:

We have to take care of everything, but with a slightly better API.

Pros:

- Less complex than vanilla DOM manipulation.

Cons:

- Doesn't enforce any sort of separation of concerns.
- Provides no means of composition.


### Backbone.js

Note:

Backbone is an MVW framework that came out in 2010. It gives structure to web applications by providing models, collections, views, and some ways of connecting it all to your backend APIs.


<!-- .slide: data-type="codes split two-column" -->
```javascript
var Greeting = Backbone.View.extend({
  initialize: function () {
    this.listenTo(
      this.model, 'change', this.render
    );
  },
  render: function () {
    // ...
  }
});
```

```javascript
var Person = Backbone.Model.extend({
  // ...
});
```

```javascript
var view = new Greeting({
  model: new Person()
});

view.render().$el.appendTo('body');
```


<iframe style="height: 480px; width: 480px;" scrolling="no" src="//codepen.io/unindented/embed/pJGJBe/?height=480"></iframe>

Note:

While it gives us the architecture for separating UI code from models, it leaves the synchronization between the two up to us. We do get events when changes occur to the UI or our models, but everything else is our responsibility.

Pros:

- Introduces separation of concerns.
- You can read the source code in an evening.

Cons:

- Models are special objects.
- Doesn't have digest cycles, causing unnecessary re-renders.
- Provides no means of composition.



<!-- .slide: data-background="rgb(0, 75, 80)" -->

## Data Binding


<!-- .slide: data-background-image="assets/change-kvo.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

Having to manually figure out re-rendering when app state changes is one of the major sources of incidental complexity in JavaScript apps.

With data binding, when your models change, only the parts of the app that use it will update. Two-way data binding links UI and models both ways, so the UI updates when models change, and models update when user interacts with the UI.

The framework is in control of both your UI and your models, so it takes care of most things.


### Knockout.js

Note:

Knockout is an MVVM framework that came out in 2010. It was one of the first to include two-way data binding.


<iframe style="height: 480px; width: 480px;" scrolling="no" src="//codepen.io/unindented/embed/rONRRX/?height=480"></iframe>

Note:

Pros:

- Auto-updates UI when models change.

Cons:

- Models are special objects.
- DSL for data binding.
- Debugging is tricky.


### Ember.js

Note:

Ember is an MVC framework that came out in late 2011. Originally it was a rewrite of another framework called SproutCore, but they ended up renaming it.


<iframe style="height: 480px; width: 480px;" scrolling="no" src="//codepen.io/unindented/embed/xwxepq/?height=480"></iframe>

Note:

Pros:

- Auto-updates UI when models change.

Cons:

- Models are special objects.
- DSL for data binding.
- Debugging is tricky.



<!-- .slide: data-background="rgb(0, 75, 80)" -->

## Dirty Checking


<!-- .slide: data-background-image="assets/change-watch.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

Dirty checking also aims to solve the problem of having to manually re-render when things have changed.

When you render a value, the framework creates a watcher for that value. After that, whenever anything happens in your app, the framework checks if the value in that watcher has changed from the last time. If it has, it re-renders the value in the UI.


### AngularJS

Note:

AngularJS is a MVW framework that came out in 2009. It is backed by Google, although they only use it for the *YouTube on PS3* app.


<iframe style="height: 480px; width: 480px;" scrolling="no" src="//codepen.io/unindented/embed/eNxNbG/?height=480"></iframe>

Note:

Pros:

- Auto-updates UI when models change.
- Uses plain data structures as models.

Cons:

- DSL for data binding.
- Debugging is tricky.



<!-- .slide: data-background="rgb(0, 75, 80)" -->

## Virtual DOM


<!-- .slide: data-background-image="assets/change-vdom-1.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

A virtual DOM is a data structure composed of plain objects and arrays that represents a real DOM object graph. A separate process then takes that virtual DOM and creates the corresponding real DOM elements that are shown on the screen.


<!-- .slide: data-background-image="assets/change-vdom-2.svg" data-background-position="50% 40%" data-background-size="75%" -->

Note:

Whenever a change occurs, a new virtual DOM is created from scratch. The previous representation is diffed with the new one, to get the set of changes, and then those changes are applied to the real DOM.


### React

Note:

Whenever you call `setState` on a component, it will trigger a re-render for that component and all of its children.

You can be more granular by overriding `shouldComponentUpdate` and comparing the previous and current states yourself, but, with complex data structures, this can be costly and error-prone.


<iframe style="height: 480px; width: 480px;" src="//codepen.io/unindented/embed/oXmjMN/?height=480"></iframe>

Note:

Pros:

- Auto-updates UI when anything changes.
- Uses plain data structures as models.

Cons:

- Updates everything by default.


### Om

Note:

By using immutable data structures, the comparison between previous and current states is trivial, so we can easily mark parts of the component tree as unchanged.

Pros:

- Auto-updates UI when anything changes.
- Uses immutable data structures as models.
- Updates only things that changed.



## Popularity

Note:

We all know that frontend development is fad-driven. Let's see what are the most popular frameworks.


<!-- .slide: data-background-image="assets/backbone-knockout-ember.svg" data-background-position="50% 40%" data-background-size="75%" -->


<!-- .slide: data-background-image="assets/backbone-knockout-ember-react.svg" data-background-position="50% 40%" data-background-size="75%" -->


<!-- .slide: data-background-image="assets/backbone-knockout-ember-react-angular.svg" data-background-position="50% 40%" data-background-size="75%" -->


<!-- .slide: data-background-image="assets/backbone-knockout-ember-react-angular-jquery.svg" data-background-position="50% 40%" data-background-size="75%" -->



## Conclusion


- Key-value observation
- Dirty checking
- Virtual DOM

Note:

- UI development is all about managing state.
- Managing it manually is a mess.
- KVO entangles app code with observables.
- Dirty checking entangles app code with weird constructs.
- Virtual DOM needs a signal to say anything may have changed.

The beauty of React lies in the combination of the virtual DOM with easily composable components.


<!-- .slide: data-background-image="assets/react-native-ios.png" data-background-position="50% 40%" data-background-size="75%" -->


<!-- .slide: data-background-image="assets/react-native-android.png" data-background-position="50% 40%" data-background-size="75%" -->


<!-- .slide: data-background-image="assets/react-blessed.png" data-background-position="50% 40%" data-background-size="75%" -->
