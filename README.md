ember-hammer
============
ember-hammer is a neat interface for defining [Hammer.js](https://github.com/EightMedia/hammer.js) gestural behaviour in your [Ember.js](http://www.emberjs.com) Views. It is easy to use and lightweight.

Note: Hammer.js 2.x support is currently experimental.

##Example

```javascript
/* ES6 Modules Example */
import Ember from 'ember';

export default Ember.View.extend({
  hammerOptions: {
    swipe_velocity: 0.5
  },
  gestures: {
    swipeLeft: function (event) {
      // do something like send an event down the controller/route chain
      return false; // return `false` to stop bubbling
    }
  }
});

/* Globals Example */
App.SomeView = Ember.View.extend({
  hammerOptions: {
    swipe_velocity: 0.5
  },
  gestures: {
    swipeLeft: function (event) {
      // do something like send an event down the controller/route chain
      return false; // return `false` to stop bubbling
    }
  }
});
```

##Usage

###With ember-cli

In your project directory (generated by ember-cli), run the following commands in your terminal:

    $ bower install --save hammerjs#1.1.3
    $ bower install --save ember-hammer

In your `Brocfile.js` (which should be in the root of your project directory), before `module.exports = app.toTree();`, add the following lines:

    app.import('bower_components/hammerjs/hammer.js');
    app.import('bower_components/ember-hammer/ember-hammer.js');

That should be it. You'll now be able to define a `gestures` object in your views.

###With globals

First, include the `ember-hammer.js` file into your asset delivery pipeline (ideally this should include minification, concatenation and gzipping facilities). ember-hammer should be included after Ember.js and Hammer.js.

###Once included

Next, define a `gestures` object in any view that you'd like to enable gestural behaviour for. Inside this object, define any Hammer.js gestural event name as a key, with the callback function to be executed as the value.

```javascript
gestures: {
    swipeLeft: function (event) {
      // do something like send an event down the controller/route chain
      return false; // return `false` to stop bubbling
    },
    swipeRight: function (event) {
        /* ... */
    }
}
```

See the full example at the top of the README.

###Passing options to Hammer.js

You can optionally define a `hammerOptions` object inside your view to specify any specific options that should be passed into Hammer.js. Options defined inside the `hammerOptions` object are specific to that view.

If you'd like to set global options for all instances of Hammer.js (applicable to all views), you can use `emberHammerOptions.hammerOptions`. See the section on emberHammerOptions below.

###Event Callback Function

The callback function is passed a single `event` argument, which is provided by Hammer.js.

The `this` context of the callback will be set to the view object, so you can access any methods on the view that you may need to get the desired behaviour.

###Event bubbling

Gestural events bubble up the DOM tree, so if you'd like to catch an event and cancel bubbling, just return `false` from your callback.

###Ember.EventDispatcher

Assuming you'll be using ember-hammer (and therefore Hammer.js) to manage touch-based gestural behaviour in your application, there is no point in having Ember's EventDispatcher listen to touch events. By default, ember-hammer will prevent EventDispatcher from listening to the following touch events:

1. `touchstart`
1. `touchmove`
1. `touchstop`
1. `touchcancel`

This brings a significant performance benefit.

You can modify this behaviour by setting `emberHammerOptions.ignoreEvents` to an array of event names EventDispatcher shouldn't bind to. Set this option to an empty array to disable this behaviour.

####ember-fastclick

If you are using ember-hammer with ember-fastclick, you will need to disable this behaviour by setting `emberHammerOptions.ignoreEvents` to an empty array. ** E.g. `window.emberHammerOptions = { ignoreEvents: [] };`

###Setting emberHammerOptions (settings / global options)

You can set the global options for ember-hammer by defining an object called `emberHammerOptions` on the `window` object. It is important that the object is defined **before** `ember-hammer.js` is loaded. A suitable place might be in an inline script tag in the head section of your document.

Example:

```javascript
window.emberHammerOptions = { 
    ignoreEvents: ['touchmove', 'touchstart', 'touchend', 'touchcancel'],
    hammerOptions: {
        swipe_velocity: 0.5
    }
};
```

`emberHammerOptions.hammerOptions` will be passed into every instance of Hammer.js. You can override these options and set additional ones for a specific `Ember.View` by setting the `hammerOptions` key inside the view object. See the relevant section above for more information.

##License

Please see the `LICENSE` file for more information.
