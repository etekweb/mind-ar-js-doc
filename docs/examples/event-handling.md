---
id: events-handling 
title: Events Handling
sidebar_label: Events Handling
---

import useBaseUrl from '@docusaurus/useBaseUrl';

:::note
This section requires basic knowledge of web development
:::

## Try it out
To try this example, you need to run on desktop browser and open the development console to see the logs. <a href={useBaseUrl('/samples/events.html')} target="_blank">Live Demo</a>

We will go through the available events one by one in the following sub-sections. 

### Complete Source
```
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@0.3.1/dist/mindar.prod.js"></script>
    <script>
      document.addEventListener("DOMContentLoaded", function() {
	const sceneEl = document.querySelector('a-scene');
	const arSystem = sceneEl.systems["mindar-system"];
	const exampleTarget = document.querySelector('#example-target');
	const examplePlane = document.querySelector('#example-plane');

	const startButton = document.querySelector("#example-start-button");
	const stopButton = document.querySelector("#example-stop-button");
	const stopARButton = document.querySelector("#example-stop-ar-button");

	startButton.addEventListener('click', () => {
	  console.log("start");
	  arSystem.start(); // start AR 
	});
	stopButton.addEventListener('click', () => {
	  arSystem.stop(); // stop AR and video
	});
	stopARButton.addEventListener('click', () => {
	  arSystem.stopAR(); // stop AR only, but keep video
	});

	// arReady event triggered when ready
	sceneEl.addEventListener("arReady", (event) => {
	  // console.log("MindAR is ready")
	});

	// arError event triggered when something went wrong. Mostly browser compatbility issue
	sceneEl.addEventListener("arError", (event) => {
	  // console.log("MindAR failed to start")
	});

	// detect target found
	exampleTarget.addEventListener("targetFound", event => {
	  console.log("target found");
	});

	// detect target lost
	exampleTarget.addEventListener("targetLost", event => {
	  console.log("target lost");
	});

	// detect click event
	examplePlane.addEventListener("click", event => {
	  console.log("plane click");
	});
      });
    </script>
  </head>
  <body>
    <div>
      <button id="example-start-button">Start</button>
      <button id="example-stop-button">Stop</button>
      <button id="example-stop-ar-button">Stop AR</button>
    </div>
    <a-scene mindar="imageTargetSrc: https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@0.3.1/examples/assets/card-example/card.mind; autoStart: false;" embedded color-space="sRGB" renderer="colorManagement: true, physicallyCorrectLights" vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
      <a-camera position="0 0 0" look-controls="enabled: false" cursor="fuse: false; rayOrigin: mouse;" raycaster="far: 10000; objects: .clickable"></a-camera>

      <a-entity id="example-target" mindar-image-target="targetIndex: 0">
	<a-plane id="example-plane" class="clickable" color="blue" opaciy="0.5" position="0 0 0" height="0.552" width="1" rotation="0 0 0"></a-plane>
      </a-entity>
    </a-scene>
  </body>
</html>
```

## arSystem

The first thing to introduce is the `arSystem` component. It's embedded inside `a-scene` and you can get the object by the following:

```
const sceneEl = document.querySelector('a-scene');
const arSystem = sceneEl.systems["mindar-system"];
```

`arSystem` provides 3 api call

```
arSystem.start(); // start AR 
arSystem.stop(); // start AR and camera feed
arSystem.stopAR(); // stop AR only, but keep camera feed 
```

By default, AR engine will start immediately, but you can disable the auto start by giving a param `autoStart: false` inside `<a-scene>`

## Events

MindAR will fire the events when happen:

### `arReady`
After `arSystem.start()`, or autostart, AR engine needs to boot up, when it's ready, this event will be fired up. You can listen to this event throught the scene element

```
const sceneEl = document.querySelector('a-scene');
sceneEl.addEventListener("arReady", (event) => {
  // console.log("MindAR is ready")
});
```

### `arError`
Sometimes, AR engine might be failed to start. There could be many reasons, but one most likely reason is camera failed to start. When this happens, this event will be fired up.

```
const sceneEl = document.querySelector('a-scene');
sceneEl.addEventListener("arError", (event) => {
  // console.log("MindAR failed to start")
});
```

### `targetFound` and `targetLost`
This events are fired up when the image target is detected/lost. You can listen to these events through the `<a-entity>`

```
// detect target found
const exampleTarget = document.querySelector('#example-target');
exampleTarget.addEventListener("targetFound", event => {
  console.log("target found");
});

// detect target lost
exampleTarget.addEventListener("targetLost", event => {
  console.log("target lost");
});

<a-entity id="example-target" mindar-image-target="targetIndex: 0">
</a-entity>

```

### `click`
When you want to do inteaction with the content, one thing you likely want to detect is when the user click/touch a certain elements. Actually, this is `AFRAME` stuff, but we'll also included here for reference.

First, you need to include the following `cursor` and `raycaster` in the `<a-camera>` element like this:

```
<a-camera position="0 0 0" look-controls="enabled: false" cursor="fuse: false; rayOrigin: mouse;" raycaster="far: 10000; objects: .clickable"></a-camera>

``` 

and then in the object that you want to detect, add a class `clickable`. Actually, it doesn't mean to be `clickable`, but the same as what you specified in the `raycaster` above.

```
<a-plane id="example-plane" class="clickable" color="blue" opaciy="0.5" position="0 0 0" height="0.552" width="1" rotation="0 0 0"></a-plane>

```

Then, it's ready. You can listen to the click event like this:

```
// detect click event
const examplePlane = document.querySelector('#example-plane');
examplePlane.addEventListener("click", event => {
  console.log("plane click");
});
```

## Wrapping up

Cool, you have now basically learnt everything about MindAR. It should gives you enough tool to do some very cool applications! 