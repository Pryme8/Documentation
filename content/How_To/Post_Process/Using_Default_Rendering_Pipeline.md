---
PG_TITLE: How To Use the Default Rendering Pipeline
---

# Introduction

You can find a complete example of this pipeline in our playground : [https://www.babylonjs-playground.com/#Y3C0HQ#146](https://www.babylonjs-playground.com/#Y3C0HQ#146)

The default rendering pipeline provides visual improvements to enhance the output of your scene:
* Antialiasing (MSAA and FXAA)
* Sharpening
* Depth of field
* Bloom
* Image processing including:
 * Vignette effect
 * Contrast
 * Exposure
 * Color curves
 * Color grading
 * Tone mapping
* Chromatic Aberration
* Grain

# Creating the rendering pipeline

You just have to create an instance of BABYLON.DefaultRenderingPipeline
```
var pipeline = new BABYLON.DefaultRenderingPipeline(
    "default", // The name of the pipeline
    true, // Do you want HDR textures ?
    scene, // The scene instance
    [camera] // The list of cameras to be attached to
);
```

# Customizing

## Antialiasing
The MSAA antialiasing (only supported in webGL 2.0 browsers) effect is off by default (set to 1) but can be turned on to 4x with:

```
pipeline.samples = 4;
```

and the FXAA antialiasing effect can be set using:

```
pipeline.fxaaEnabled = true;
```

## Sharpening
Sharpening can be enabled with:

```
pipeline.sharpenEnabled = true;
```
To increase the intensity of the effect modify:
```
pipeline.sharpen.edgeAmount = 0.9;
```
The amount of the original image in the output can be set with (Setting this to 0 will produce edge detection output):
```
pipeline.sharpen.colorAmount = 0.0;
```

## Depth of field
You can turn the depth of field effect on and off with:

```
pipeline.depthOfFieldEnabled = true;
```

Set the strength of blur with (Higher may affect performance):

```
pipeline.depthOfFieldBlurLevel = BABYLON.DepthOfFieldEffectBlurLevel.Low;
```

Furthermore, you can control the settings of the effect with the following parameters:
```
pipeline.depthOfField.focusDistance  = 2000; // distance of the current focus point from the camera in millimeters considering 1 scene unit is 1 meter
pipeline.depthOfField.focalLength  = 50; // focal length of the camera in millimeters
pipeline.depthOfField.fStop  = 1.4; // aka F number of the camera defined in stops as it would be on a physical device
```
[Demo](https://www.babylonjs-playground.com/#8F5HYV#9)

## Bloom
You can turn the bloom effect on and off with:

```
pipeline.bloomEnabled = true;
```

The bloom luminance threshold, impact of the bloom, kernel size and scale can be controlled with the following:
```
pipeline.bloomThreshold = 0.8;
pipeline.bloomWeight = 0.3;
pipeline.bloomKernel = 64;
pipeline.bloomScale = 0.5;
```


## Image processing effect
You can turn the image processing effect on and off with:

```
pipeline.imageProcessingEnabled = true;
```

You can also control individual image processing subeffects. To get more info about the ImageProcessing postprocess, please read the following [tutorial](/How_To/how_to_use_postprocesses#imageprocessing).

## Chromatic Aberration
You can turn the effect on and off with:

```
pipeline.chromaticAberrationEnabled = true;
```

Furthermore, you can control the distance of color channel separation with:
```
pipeline.chromaticAberration.aberrationAmount = 300;
```

To modify the strength of the effect based on the distance from the center of the screen:
```
pipeline.chromaticAberration.radialIntensity = 3;
```

To modify the direction the aberration the direction can be set:
```
var rotation = Math.PI;
pipeline.chromaticAberration.direction.x = Math.sin(rotation)
pipeline.chromaticAberration.direction.y = Math.cos(rotation)
```
Note: If both these values are set to 0 the direction will be towards the center of the screen.

## Grain
You can turn the effect on and off with:

```
pipeline.grainEnabled = true;
```

Furthermore, you can control intensity of the grain with:
```
pipeline.grain.intensity = 10;
```

To set grain to animate on every frame set:
```
pipeline.grain.animated = value;
```

# Example

You may be interested by this code example using default values, ready to be copy-pasted into your application:

```javascript
    // https://doc.babylonjs.com/api/classes/babylon.defaultrenderingpipeline
    var defaultPipeline = new BABYLON.DefaultRenderingPipeline(
        "DefaultRenderingPipeline",
        true, // is HDR?
        scene,
        scene.cameras
    );
    if (defaultPipeline.isSupported) {
        /* imageProcessing */
        defaultPipeline.imageProcessingEnabled = true; //true by default
        if (defaultPipeline.imageProcessingEnabled) {
            defaultPipeline.imageProcessing.contrast = 1; // 1 by default
            defaultPipeline.imageProcessing.exposure = 1; // 1 by default
            /* color grading */
            defaultPipeline.imageProcessing.colorGradingEnabled = false; // false by default
            if (defaultPipeline.imageProcessing.colorGradingEnabled) {
                // using .3dl (best) :
                defaultPipeline.imageProcessing.colorGradingTexture = new BABYLON.ColorGradingTexture("textures/LateSunset.3dl", scene);
                // using .png :
                /*
                var colorGradingTexture = new BABYLON.Texture("textures/colorGrade-highContrast.png", scene, true, false);
                colorGradingTexture.wrapU = BABYLON.Texture.CLAMP_ADDRESSMODE;
                colorGradingTexture.wrapV = BABYLON.Texture.CLAMP_ADDRESSMODE;                
                defaultPipeline.imageProcessing.colorGradingTexture = colorGradingTexture;
                defaultPipeline.imageProcessing.colorGradingWithGreenDepth = false;
                */
            }
            /* color curves */
            defaultPipeline.imageProcessing.colorCurvesEnabled = false; // false by default
            if (defaultPipeline.imageProcessing.colorCurvesEnabled) {
                var curve = new BABYLON.ColorCurves();
                curve.globalDensity = 0; // 0 by default
                curve.globalExposure = 0; // 0 by default
                curve.globalHue = 30; // 30 by default
                curve.globalSaturation = 0; // 0 by default
                curve.highlightsDensity = 0; // 0 by default
                curve.highlightsExposure = 0; // 0 by default
                curve.highlightsHue = 30; // 30 by default
                curve.highlightsSaturation = 0; // 0 by default
                curve.midtonesDensity = 0; // 0 by default
                curve.midtonesExposure = 0; // 0 by default
                curve.midtonesHue = 30; // 30 by default
                curve.midtonesSaturation = 0; // 0 by default
                curve.shadowsDensity = 0; // 0 by default
                curve.shadowsExposure = 0; // 0 by default
                curve.shadowsHue = 30; // 30 by default
                curve.shadowsDensity = 80;
                curve.shadowsSaturation = 0; // 0 by default;
                defaultPipeline.imageProcessing.colorCurves = curve;
            }
        }
        /* bloom */
        defaultPipeline.bloomEnabled = false; // false by default
        if (defaultPipeline.bloomEnabled) {
            defaultPipeline.bloomKernel = 64; // 64 by default
            defaultPipeline.bloomScale = 0.5; // 0.5 by default
            defaultPipeline.bloomThreshold = 0.9; // 0.9 by default
            defaultPipeline.bloomWeight = 0.15; // 0.15 by default
        }
        /* chromatic abberation */
        defaultPipeline.chromaticAberrationEnabled = false; // false by default
        if (defaultPipeline.chromaticAberrationEnabled) {
            defaultPipeline.chromaticAberration.aberrationAmount = 30; // 30 by default
            defaultPipeline.chromaticAberration.adaptScaleToCurrentViewport = false; // false by default
            defaultPipeline.chromaticAberration.alphaMode = 0; // 0 by default
            defaultPipeline.chromaticAberration.alwaysForcePOT = false; // false by default
            defaultPipeline.chromaticAberration.enablePixelPerfectMode = false; // false by default
            defaultPipeline.chromaticAberration.forceFullscreenViewport = true; // true by default
        }
        /* DOF */
        defaultPipeline.depthOfFieldEnabled = false; // false by default
        if (defaultPipeline.depthOfFieldEnabled && defaultPipeline.depthOfField.isSupported) {
            defaultPipeline.depthOfFieldBlurLevel = 0; // 0 by default
            defaultPipeline.depthOfField.fStop = 1.4; // 1.4 by default
            defaultPipeline.depthOfField.focalLength = 50; // 50 by default, mm
            defaultPipeline.depthOfField.focusDistance = 2000; // 2000 by default, mm
            defaultPipeline.depthOfField.lensSize = 50; // 50 by default
        }
        /* FXAA */
        defaultPipeline.fxaaEnabled = false; // false by default
        if (defaultPipeline.fxaaEnabled) {
            defaultPipeline.fxaa.samples = 1; // 1 by default
            defaultPipeline.fxaa.adaptScaleToCurrentViewport = false; // false by default
        }
        /* glowLayer */
        defaultPipeline.glowLayerEnabled = false;
        if (defaultPipeline.glowLayerEnabled) {
            defaultPipeline.glowLayer.blurKernelSize = 16; // 16 by default
            defaultPipeline.glowLayer.intensity = 1; // 1 by default
        }
        /* grain */
        defaultPipeline.grainEnabled = false;
        if (defaultPipeline.grainEnabled) {
            defaultPipeline.grain.adaptScaleToCurrentViewport = false; // false by default
            defaultPipeline.grain.animated = false; // false by default
            defaultPipeline.grain.intensity = 30; // 30 by default
        }
        /* MSAA */
        defaultPipeline.samples = 1; // 1 by default
        /* sharpen */
        defaultPipeline.sharpenEnabled = false;
        if (defaultPipeline.sharpenEnabled) {
            defaultPipeline.sharpen.adaptScaleToCurrentViewport = false; // false by default
            defaultPipeline.sharpen.edgeAmount = 0.3; // 0.3 by default
            defaultPipeline.sharpen.colorAmount = 1; // 1 by default
        }
    }
```

As usual, a [playground is available](https://www.babylonjs-playground.com/#ECI2Q0#5).

# Further Reading

[How To Use PostProcess](How_To/How_to_use_PostProcesses)