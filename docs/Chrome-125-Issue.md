# Chrome 125 Issue

With the latest release, Chrome introduces an issue related to RTCRtpSender.replaceTrack() and MediaStreamTrackGenerator tracks.

It gets stuck during transmission when you try to replace the current MediaStreamTrack from the webcam on the MediaStreamTrackGenerator tracks.

Our SDK is using MediaStreamTrackGenerator for Chromium-based browsers to generate the output stream with applied effects.

Depending on the implemented logic of SDK usage, it may affect some of your users.


## Workarunds


### Option 1

Use an intermediate canvas to capture the stream:

 - Create a canvas with the size of the webcam video
 - Pass this canvas to the SDK by using sdk.toCanvas(canvas)
 - Grab the stream from the canvas: canvas.captureStream(30)

### Option 2

Don't pass the real stream from the webcam to RTCPeerConnection. Always use stream from sdk.getStream(), if you would like to disable all efeects you can use sdk.stop()
In this scenario, RTCRtpSender.replaceTrack() should work correctly.

### Option 3

Do not use RTCRtpSender.replaceTrack() for track replacement.