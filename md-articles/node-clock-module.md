# A Node.js Clock Module for GUI Applications

As a platform for server applications there isn't anything quite like node.js (or io.js). However node has other uses in fact it is now used by several development environments to run and check javascript and do build tasks. It can also be used to construct command line applications. In addition to all that flexibility there are now quite a few GUI (graphical user interface) toolkits available for the node platform which are capable of being used to craft professional quality desktop applications. These desktop apps retain all of node's extensive networking capabilities allowing the development of highly network connectable desktop applications.

This article will demonstrate how to build a clock module for node which could easily be used within a desktop application framework like [nw.js (formerly node-webkit)](http://nwjs.io/) or [Electron](https://electronjs.org/) or with [node's GTK bindings]( https://github.com/zcbenz/node-gui), [QT bindings]( https://github.com/arturadib/node-qt), [tk bindings]( https://github.com/sidorares/ntk) or [X11 bindings]( https://github.com/sidorares/node-x11) as well.

## Building the Module

We will need a few standard node modules for our clock so lets start with those.

```javascript
var events =require('events');
var util =require('util');
```

The first of these modules is the “events” module. This module allows an object like our clock to “emit” events and to enable our application to run code when that event is emitted. If you are familiar with using Javascript in a web browser, you know that HTML elements “emit” events in response to things a user, or the browser does (like the user clicking on a button or the browser loading a document). In order to do something useful with those events like `click`,`mouseover` or `blur` your code has to listen for those events. A listener is basically a function which will execute once an event is fired.

In node we don't have an HTML DOM (document object model) but we do have Javascript objects which do things, and those things they do might take some time or as with a clock happen on a regular ongoing basis. Listening for events, and executing code when the events occur is a way to handle this. The networking in node uses this to great effect and the events library makes it possible.

The second component we need is the `util` module which contains many general functions useful for working in node. In this case we are interested in the `inherits` function which allows our clock to inherit from the Event object. This is roughly the equivalent to inheritance in many object oriented languages.

```javascript
function Clock(){
  events.EventEmitter.call(this);
}
util.inherits(Clock,events);
```

The code above is our clock constructor. The first thing we do is call the constructor of our EventEmitter superclass within the scope of “this” which is our Clock subclass. This call actually isn't strictly necessary for the EventEmitter class as it's constructor doesn't actually do anything, but good inheritance practice in node is to call it anyway.If we were subclassing an object which did do something in it's constructor, then we would have to call it in our subclass. Every argument after the first argument in the call method is an argument to the function/constructor you are calling. So if EventEmitter required a single argument the “call” method would look something like this.

```javascript
events.EventEmitter.call(this,'foo');
```

Luckily we aren't concerned with any arguments to the superclass in this example. The `util` module then allows Clock to inherit from `EventEmitter` by copying all of `EventEmitter` methods and properties over to our clock.

The `EventEmitter` can be used on it's own as well and can be very useful in alleviating something which `node.js` and javascript coders in other platforms run into called “callback hell”. Where asynchronous code must be done with nested callback functions which seemingly go on forever.Events aren't a complete substitute for callbacks, but they are very useful for streamlining your code and making things much easier to scan and read. Our clock module could use callbacks but works really well with events.

## The Timer

We don't have to go to a lot of effort to make our clock run. We simply need to use Javascript's built in `setInterval` function to put our clock into motion. First we need to make sure we have a reference to our clock instance inside of the function closure executed by `setInterval`. Setting a variable called `self` which refers to `this` will do that nicely.

```javascript
function Clock(){
  events.EventEmitter.call(this);
  var self = this;
  setInterval(function(){
  },1000);
}
util.inherits(Clock,events);
```

This will execute whatever is inside our callback function body once every second. Given that `setInterval` asks for timing in milliseconds in it's second argument a value of `1000` will give us a timing interval of one second. The next thing to do is set up a function which will give you the current correctly formatted time every time it's called.

The code for this method of our clock comes from a tutorial by Matt Doyle called “Creating a Javascript Clock” available [here]( http://www.elated.com/articles/creating-a-javascript-clock/). We simply need to adapt Matt's code to work within our node based clock.

```javascript
Clock.prototype.readout= function(){
  var currentTime = new Date();
  var currentHours = currentTime.getHours();
  var currentMinutes = currentTime.getMinutes();
  var currentSeconds = currentTime.getSeconds();
  currentMinutes = ( currentMinutes < 10 ? "0" : "" ) +currentMinutes;
  currentSecond s= ( currentSeconds < 10 ? "0" : "" ) +currentSeconds;
  vartimeOfDay = ( currentHours < 12 ) ? "AM" : "PM";
  currentHours = ( currentHours > 12 ) ? currentHours - 12 : currentHours;
  currentHours = ( currentHours == 0 ) ? 12 : currentHours;
  returncurrentHours + ":" + currentMinutes + ":" + currentSeconds + " " + timeOfDay;
};
```

This will produce an `HH::MM:SS` 12 hour time format with *AM* or *PM* suffix. This code could be reworked to produce any date format or a date/time module could be used which might operate similar to the way many other languages do dates using a format string you provide but this format is good enough for our clock for now.

## Making it Tick and Tock

Now we need to hookup this new readout method to our timer so that when the constructor is called it starts our clock.

```javascript
function Clock(){
  events.EventEmitter.call(this);
  var self = this;
  setInterval(function(){
    self.emit('update',self.readout());
  },1000);
}
```

Here we are “emitting” an event called `update` which our application code can listen for. Whenever the `update` event fires, it sends the output of our `readout` method to whatever function is set to execute when the event fires. Which of course as I noted above, will be called every second.

The only thing left to do is to make this module official and return a singleton clock object when it is required.

```javascript
module.exports = new Clock()
```

## Displaying the Time

Now that our clock is finished lets get it hooked up to our node application. You should have the clock code stored in the `node_modules` directory within your application's directory. We can call it `clock.js`.

```javascript
clock = require('clock');
```

In this case I have chosen to use `nw.js` for my application platform, so my javascript file is actually included using a normal HTML script tag within the `index.html` file `nw.js` is set to open when it is launched. The index file also has JQuery included so you will see some JQuery code intended to put our clock output into an HTML element.

Telling you how nw.js works is beyond the scope of this article but you can find a lot of tutorials on how to get started by doing some searching. Suffice it to say we could display the time from our clock module whatever way was appropriate for the application framework we are using.

```javascript
clock.on('update',function(time){
  //jQuery code to put our time into an HTML element in the interface
  // This could be any kind of code to display to the user.
  var el = $('#clock').html(time);
});
```

This code is telling the clock to execute a function which displays the time whenever the `update` event is fired. You will then see the time ticking by by the second as this code refreshes in your user interface.

Calling our `readout()` method directly will get us a string timestamp at the time it is called.

## Conclusion

You now have a fully functional clock which updates every second for your GUI application. Check out the node based GUI modules and have a good err..... time with it.
