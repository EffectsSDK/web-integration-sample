# Features Usage Examples

## General Details

- All SDK calls have to be done only after sdk.onReady callback happens

```
   sdk.onReady = () => {
           console.log('SDK is ready let\'s run it');
           sdk.run();
   }

```

- When you are changing the camera stream you need to do following:

```
   sdk.stop();
   sdk.clear();
   sdk.useStream(stream);

   //after that sdk.onReady ccallback will happen again
```

- SDK doesn't keep the configuration state inside, you should do it by yourself. So after the camera stream will be changed you need to restore SDK state (by configuring again as needed)

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

You can changa the segmentation preset on the fly:

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

## Components system

Imagine an artist drawing a animation. He draws individual elements on transparent sheets, overlaying them layer by layer, to get the finished frame. We have implemented a similar concept with Components system. Component - is the frame layer, that contain the specialized element.

You can create the component you need:

```
const newComponent_1 = sdk.createComponent({
   component: "stickers",
   options: { capacity: 16, duration: 3500 },
})
```

Available components: <i>"overlay_screen", "stickers", "lowerthird_1", "lowerthird_2", "lowerthird_3", "lowerthird_4", "lowerthird_5"</i>.
Add the created component to the frame in the order you want with unique ID that is needed to access the component. In this case <i>newComponent_2</i> will be dysplayed on top of the <i>newComponent_1</i>

```
sdk.addComponent(newComponent_2, "component_id_2");
sdk.addComponent(newComponent_1, "component_id_1");
...
newComponent_1.show()
// equivalent records
sdk.components.component_id_1.show()

sdk.components["component_id_2"].show()
```

Each component has an options and methods for getting/setting them (<i>setOptions(), getOptions()</i>), <i> show()/hide()</i> methods
and lifecycle hooks: <i>onBeforeShow, onAfterShow, onBeforeHide, onAfterHide</i>:

```
component.onBeforeShow(() => {
   console.log("I will be called before showing the component")
})
```

Below we will analyze in more detail the different types of components.

### How to use Overlays

To use Overlays, you need to create new component by selecting the "overlay_screen" type and add it to the list of components. Note that is this case may be useful component lifecycle hooks: for example, we can stop effects pipeline after <i>overlay</i> is shown, and run pipeline before the component is hidden:

```
const overlayScreen = sdk.createComponent({
   component: "overlay_screen",
   options: {}
});

sdk.addComponent(overlayScreen, "overlay");

sdk.components.overlay.onAfterShow(() => sdk.enablePipelineSkipping());
sdk.components.overlay.onBeforeHide(() => sdk.disablePipelineSkipping());

...

sdk.components.overlay.show()
```

### How to use Stickers

To use Stikers, you need to create new component by selecting the "stickers" type and pass necessary options. Then add it to the list of components:

```
const stickers = sdk.createComponent({
   component: "stickers",
   options: {
      capacity: 16,
      duration: 3500
   },
});

sdk.addComponent(stickers, "my_stickers_component");
```

The Sticker-component has a StickerStore, the capacity of which is set by options. It allows you to store a certain number of loaded textures that can be rendered without delay. The StickerStore is a queue, each texture that has been preempted is destroyed. You can add a texture to the StickerStore by the <i>setOptions()</i> method. Loading texture is async process. There are two ways to handle texture loading promise (for example for the loading indication): add special object <i>promise</i> to the options argument:

```
sdk.components.my_stickers_component.setOptions({
   sticker: {
      url: 'https://my.stickers.url/sticker.mp4',
      promise: {
         resolve: () => console.log('success'),
         reject: () => console.log('badluck')
      }
   }
});
```

or use Stickers-component hooks <i>onLoadSucccess/onLoadError</i>:

```
sdk.components.my_stickers_component.onLoadSucccess(() => {
   console.log('success');
});
```

After that you can add loaded sticker to the frame by the setting option <i>id</i> (Note that sticker unique <i>id</i> is the <i>url</i>, taht was used for the texture loading):

```
sdk.components.my_stickers_component.setOptions({
   id: 'https://my.stickers.url/sticker.mp4'
});
```

### How to use Lower-Thirds(LT)

First of all, you need to create a LT component by selecting the LT type and setting the required options. After that, you need to add the created LT to the list of components:

```
const lt = sdk.createComponent({ component: 'lowerthird_1', options: {
      text: {
         title: 'John Doe',
         subtitle: 'Some description'
      },
      color: {
         primary: 161271
      }
   }
});

sdk.addComponent(lt, "lowerthird");
```

In addition to the component methods, the LT has special <i>showLowerThird()</i> and <i>hideLowerThird()</i> methods that allow you to show/hide the component with an animation effect:

```
sdk.components.lowerthird.showLowerThird();
sdk.components.lowerthird.hideLowerThird();
```

## How to resize stream on the fly

```
sdk.setOutputResolution({
   width?: number,
   height?: number
})
```

## Show metrics

Control the visibility of metrics:

```
sdk.showFps()
sdk.hideFps()
```

Function for setting fps limit:

```
sdk.setFpsLimit(limit: number)
```

## How automatically switch presets

```
sdk.setSegmentationPreset(preset: "quality" | "balanced" | "speed" | "lightning")
```
