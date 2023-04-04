# Video Effects SDK - integration sample
Add real-time AI video enhancement that makes video meeting experience more effective and comfortable to your application in a few hours. 

Introducing our powerful and advanced web SDK for video communication products. With our web SDK, you can now elevate your video conferencing experience with features like background blur, virtual background, auto-framing or smart zoom, beautification, and automatic color correction.

Our background blur feature helps you maintain your privacy and professionalism during video calls by blurring out the background behind you. Virtual background takes it one step further by allowing you to replace your real background with a custom image or video of your choice.

With our auto-framing or smart zoom feature, you no longer have to worry about staying in frame during your video calls. Our SDK automatically adjusts the camera's field of view to keep you centered in the frame, even if you move around.

Our beautification feature enhances your appearance during video calls by smoothing out skin tones, removing blemishes, and adjusting lighting to create a more polished look. This feature is perfect for those who want to boost their confidence and create a professional appearance.

Finally, our automatic color correction feature adjusts the camera's color settings to improve the overall image quality, even in low-light environments or with poor quality cameras.

Overall, our web SDK is the perfect solution for those looking to take their video conferencing experience to the next level. With advanced features like background blur, virtual background, auto-framing or smart zoom, beautification, and automatic color correction, you can create a professional and polished appearance during your video calls. Try our web SDK today and elevate your video conferencing experience.

## Requirements

- Obtaining Effects SDK Customer ID
- SSL to get MediaStream from browser
- Support of WebGL 1.0

## Documentation
[SDK Documentation](https://effectssdk.com/sdk/web/docs/classes/tsvb.html)

## Demo
[Live Demo](https://effectssdk.com/sdk/demo)

For the best **quality** - disable **Frame Skipping** and use **Quality** segmentation preset

For the best **performance** – enable **Frame Skipping** and use **Speed** segmentation preset

## Obtaining Effects SDK Customer ID
Effects SDK Customer ID is required to get SDK working.

To receive a new trial Customer ID please fill in the contact form on [effectssdk.com](https://effectssdk.com/request-trial) website.

## Technical Details

- CUSTOMER_ID should be provided to the SDK constructor.
- SDK has 3 speed/quality presets (different segmentationn models).
- To improve output FPS SDK has ability to skip the frames.

## Features

- Virtual backgrounds (put image or video as a background) - **implemented**
- Use Desktop Capture as a background - **implemented**
- Background blur - **implemented**
- Beautification/Touch up my appearance - **implemented**
- Layouts - **implemented**
- Auto framing - **in progress**
- Auto color correction - **in progress**
- Color grading - **in progress**
