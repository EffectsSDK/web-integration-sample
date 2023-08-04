# Features Usage Examples

## General Details

 * All SDK calls have to be done only after sdk.onReady callback happens
 ```
    sdk.onReady = () => {
            console.log('SDK is ready let\'s run it');
            sdk.run();
    }

 ```
 * When you  are changing the camera stream you need to do following:
 ```
    sdk.stop();
    sdk.clear();
    sdk.useStream(stream);

    //after that sdk.onReady ccallback will happen again
 ```
 * SDK doesn't keep the configuration state inside, you should do it by yourself. So after the camera stream will be changed you need to restore SDK state (by configuring again as needed)

## How to use Virtual Backgrounds

Put the color on the background:
```
sdk.setBackgroundColor(0x00ff00);
sdk.setBackground('color');
```

Put the image on the background:
```
sdk.setBackground('image url');
```

Put the video on the background:
```
sdk.setBackground('video url');
```

Put the MediaStream on the background:
```
sdk.setBackground(MediaStream);
```

Change the background fit mode:
```
sdk.setBackgroundFitMode('fit');
```

Clear the background:
```
sdk.clearBackground();
```

## How to use Segmentation presets

You can  changa the segmentation preset on the fly:
```
sdk.setSegmentationPreset('quality');
```

Also you can specify which preset would work as default one:
```
// right after the initialisation
sdk.config({
          preset: 'speed'
	});

```

## How to use Background Blur

Enable blur (it will apply on the fly, so you can provide the control to users):
```
sdk.setBlur(0.6);
```

Disable blur:
```
sdk.clearBlur();
```

## How to use Beautification

Enable beautification:
```
sdk.enableBeautification();
//you can chamnge the power of effects on the fly
sdk.setBeautificationLevel(0.6);
```

Disable beautification:
```
sdk.disableBeautification();
```

## How to use Auto-Framing

Enable auto-framing (smart zoom):
```
sdk.enableSmartZoom();
//set the level of zooming
sdk.setFaceArea(0.2);
```

Disable auto-framing (smart zoom):
```
sdk.disableSmartZoom();
```


## How to use Color Correction

Enable color correction:
```
sdk.enableColorCorrector();
//change the power of effect
sdk.setColorCorrectorPower(0.5);
```

Disable color correction:
```
sdk.disableColorCorrector()
```

## How to use Lower-Thirds

## How to use Overlays

## How to resize stream on the fly

## Show metrics

## How automatically switch presets