# Best Practices


## Initialization and Preloading
  
  During the initialization stage, our SDK performs the following steps:

  - Determines which inferences are supported and loads the appropriate versions of .wasm files.
  - Understands (based on the SDK config) which features should be enabled and loads all required .tsvb model files.
  - Sequentially initializes all models in memory (as required, they cannot be initialized simultaneously).
  - Each ML feature must be authenticated by calling our license server during initialization.

  By default, the initialization of the SDK starts only when you provide the stream to it.
  In some cases, it could take a significant amount of time (up to 15-20 seconds, depending on the network and environment conditions).

  We have introduced 2 methods aimed at improving the initialization speed.

  1. [sdk.cache()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#cache)
    
  - This method retrieves all required models (.tsvb) based on the sdk.config and stores them locally in named storage.
  - You can call this immediately after the SDK instance was created.
  - All loading works asynchronously and should not affect anything else.
  - You can do this at the stage of initial loading of your application (not right before you receive the stream from the camera).
  - This will significantly decrease the subsequent loading speed of the SDK.


  2. [sdk.preload()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#preload)

  - This method preloads and initializes all required models in memory and performs necessary authentication.
  - If models are already cached (by `sdk.cache` or by browser cache), it will retrieve them from the cache.
  - You can call this immediately after the SDK instance was created, so in this case, initialization will not depend on the webcam stream gathering.
  - We recommend doing this on the page where you are working with the camera.

  3. [sdk.config()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#config)

  - This method is the central point for SDK configuration.
  - If you are using only some features and don't require others, you need to disable them.
  - If a feature is disabled, this means that we will not load assets for these features, and SDK initialization will be faster.

## Performance improvements

  We are continuously improving the performance of our SDK, gradually decreasing CPU usage and finding the balance between CPU and GPU.

  ### Segmentation on WebGPU

   The most processing-intensive function is background segmentation. We have added support for WebGPU segmentation where available.
   If WebGPU is not supported by the browser, the SDK will automatically fall back to the CPU version of segmentation.

   To enable it, configure it as follows:

    sdk.config({
        provider: 'webgpu'
    });


   ### CPU usage has a directly proportional relationship with the number of segmentations in a given period.

   To free up the CPU and improve the final FPS, you can use the following methods:

   1. [sdk.enableFrameSkipping()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#enableFrameSkipping)

   - This means that segmentation will occur every second frame. For frames where segmentation is skipped, we will use the segmentation mask from the previous frame.
   - As a result, you will achieve higher final FPS. However, there may be minor visible delays or fluctuations in the segmentation mask, especially when people in the frame are moving quickly. 

   2. [sdk.setFpsLimit()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#setFpsLimit)

   - As less frames needs to be processed in a portion of time -> less segmentation will be done -> less CPU/GPU usage
    
## Disabling the SDK when required
  
   In cases where your customers' hardware/software is insufficient to deliver seamless results with video processing while running your main application logic, you can simply disable our SDK.

   1. [sdk.stop()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#stop)

   - This will halt all processing and will bypass the original frames to the output.
   - This scenario negates any performance effects on your application.

   2. [sdk.run()](https://effectssdk.ai/sdk/web/docs/classes/tsvb.html#run)

   - At any time, you can restart the SDK.


