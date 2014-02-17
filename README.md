scrollwatcher
=============

This is a module for the browser that will emit events when the elements you specify scroll into or out of view. You get to define what is "in view."

Usage
-----

See the [example](http://jimkang.github.io/scrollwatcher/example/index.html).

First, you create the scrollwatcher object.

    var scrollWatcher = createScrollWatcher();

Then, you tell it what elements to watch.

    scrollWatcher.watchElements(
      [
        {
          selector: '.one', 
          isInViewFunction: middleThirdOfElIntersectsWithView
        },
        {
          selector: '.two', 
          isInViewFunction: middleThirdOfElIntersectsWithView
        },
        {
          selector: '.three'
          // Not specifying an isInViewFunction so scrollWatcher will default to 
          // checking to see if the center of the element is in the view.
        }
      ]
    );

Each element in the array you pass to watchElements should contain a selector string, which it will use to initially find the element.

Optionally, you can also provide isInViewFunction, a function that takes the y value of the top of the current view, the bottom of the current view, and the DOM element being watched and then returns whether or not the element should be counted as being "in view."

For example, this function defines "in view" as the middle third of the element intersecting with the view.

    function middleThirdOfElIntersectsWithView(viewTop, viewBottom, el) {
      var topThreshold = el.offsetTop + el.offsetHeight * 0.33;
      var bottomThreshold = el.offsetTop + el.offsetHeight * 0.67;
      return !(bottomThreshold < viewTop || topThreshold > viewBottom);
    }

Then, you add event listeners for elMovedIntoView and/or elMovedOutOfView. The 'detail' property of the event will be the DOM element that either moved into or out of view.

    document.addEventListener('elMovedIntoView', 
      function onElCenterInView(e) {
        // Do whatever you want in response to the element moving into view.
        e.detail.classList.add('in-spotlight');
      }
    );

    document.addEventListener('elMovedOutOfView', 
      function onElCenterOutOfView(e) {
        e.detail.classList.remove('in-spotlight');
      }
    );

Optionally, you may want to directly call respondToScroll when the page loads since scroll events don't happen when a page loads:

    scrollWatcher.respondToScroll();

License
-------

MIT.
