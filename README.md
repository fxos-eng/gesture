# gesture
Simple promise-based multi-touch gesture detection.

gesture.js defines a Gesture.detect() that returns a promise that
resolves when it detects a swipe gesture. Use it with code like this:

```
// This object defines a gesture
var swipeUp = {
  type: 'swipe',    // Swipe:
  numFingers: 2,    // with two fingers,
  startRegion: {    // from bottom 10% of the screen,
    x0: 0, y0: 0.9, x1: 1, y1: 1
  },
  endRegion: {      // up into the top 80% of the screen,
    x0: 0, y0: 0, x1: 1, y1: 0.80
  },
  maxTime: 1000,    // in less than 1 second.
};

// This is how we detect an instance of that gesture
Gesture.detect(swipeUp).then(function(details) {
  console.log('Got a swipe up gesture with details', details);
  respondToSwipeUp(details);
});
```

The returned Promise object has a cancel() method added to it. Call
that method if you want to stop listening for the gesture. Calling
cancel() will reject the promise, and this is the only reason that a
promise will be rejected once gesture detection begins.

Right now 'swipe' is the only gesture type that is supported, but the
code is designed to make it possible to add new gesture types
relatively easily.

Note that this is a single-shot gesture detector. Once it has detected
the gesture, it is done. If you want to detect more gestures, you must
call Gesture.detect() again. That is why it uses Promises instead of
firing events. This also means that it is not suitable for gestures
like pinches were we need a stream of events that we can respond to
while the pinch is in progress.
