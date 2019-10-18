Starting with Babylon.js v4.1, it is now possible to use one engine on several canvases.

To do so, you need to create "views". A view is defined by a canvas (the target) and an optional camera (the definition of the view).

**Please note that when a view is registered, the canvas used to initialized the Engine becomes a "working" canvas and is no more supposed to be displayed directly.**

## Creating a view

To create a view, you need to register the combination of a canvas and a camera to the engine:

```
let view = engine.registerView(document.getElementById("renderCanvas1"), camera1);
```

Please note that you can also call the same code with no camera:
```
let view = engine.registerView(document.getElementById("renderCanvas1"));
```

If the camera is undefined the render loop will be executed to the view without changing anything on the active scenes.

## Removing a view

To remove a previously registered view, you can run the following code:
```
engine.unRegisterView(document.getElementById("renderCanvas1"));
```

## Cameras

If a view is defined with a camera, the system will use it as the active camera to render the scene.

A view cannot be rendered if it has a defined camera and the underlying scene is using multiple active cameras(eg. scene.activeCameras.length > 0).

## Multi scenes

You can use views to render multiple cameras from the same scene or from different scenes. As you control the render loop, it is up to you to test the `engine.activeView` to determine which view is currently rendered.

So your render loop could look like:
```
let myRenderLoop = () => {
   if (engine.activeView.camera === undefined) {
       mainScene.render();
   } else if (engine.activeView.canvas === view1) {
       scene1.render();
   }
}
```

## Events

By default, scenes will capture events from the main rendering canvas (the one used to create the Engine). You can change that behavior by setting the `engine.inputElement` to the DOM element you want to use for events.

This must be done before creating a scene or if you do it after you have to run the following code:
```
scene.detachControl();
engine.inputElement = myNewElement;
scene.attachControl();
```

## Demo
You can see a live demo here: https://www.babylonjs.com/Demos/Views