# BloomPostprocess Sample
This sample shows how to use bloom post-processing filters to add a glow effect over the top of an existing scene.

## Sample Overview

Bloom post-processing emulates the visual effect of bright lights and glowing objects. It does this by extracting the brightest parts of an image to a custom render target, blurring these bright areas, and then adding the blurred result back into the original image.

Because bloom is implemented entirely as a post-process, it can easily be used over the top of any other 2D or 3D rendering techniques. In addition to the more extreme glowing effects, when used subtly it provides a useful softening that can make computer graphics look more organic and help to hide artifacts from elsewhere in your rendering. Particle systems, for example, often look better with a subtle bloom applied over the top.

## Sample Controls

This sample uses the following keyboard and gamepad controls.

| Action                                   | Keyboard Control | Gamepad Control |
| ---------------------------------------- | ---------------- | --------------- |
| Change the bloom settings                | **A**            | **A**           |
| Bloom, toggle on or off                  | **B**            | **B**           |
| Show intermediate render target contents | **X**            | **X**           |
| Exit                                     | ESC or ALT+F4    | **BACK**        |


## How the Sample Works

There are three stages to applying a bloom post-process.

- Pass 1
  - Extract the brightest parts of the scene. This uses the BloomExtract.fx pixel shader, which removes any areas darker than the specified threshold parameter value. Modifying the threshold changes the look of the bloom: higher values produce smaller and better defined glows, while smaller ones produce a softer end result. To see just the result of this first pass, press **X** until the overlay display shows "PreBloom." 
- Passes 2 and 3
  - Blur the bright areas. This is done by using the GaussianBlur.fx pixel shader—first, to blur the image horizontally, and then again to blur it vertically. The shader does multiple lookups into different parts of the source texture, and then averages the results using a Gaussian curve to weight the importance of each sample. The work is split over two passes because Shader Model 2.0 is not capable of doing enough texture lookups to give a high-quality blur along both the horizontal and vertical axis in a single pass. To see the result of these blur passes, press X until the overlay display shows "BlurredHorizontally" or "BlurredBothWays." 
- Pass 4
  - Combine the blurred result with the original image. This uses the BloomCombine.fx pixel shader, which has parameters to control how much bloom to include, how much of the original image to retain, and for adjusting the color saturation of both the bloom and original base images. Tweaking these settings can produce a wide range of visual effects.

## Extending the Sample

Rather than just choosing a fixed set of bloom settings, you could adjust these on the fly depending on what is happening in your game. Perhaps you could make the world go grayer and more blurry when the player has low health, or could increase the amount of glow for a second or two when the player gets a power up.

You can also adjust bloom settings to mimic how the human eye responds to changing light levels. If the player is in a long tunnel, turn the bloom up very high so any areas of the outside world that are visible past the end of the tunnel will burn-out to white and have a huge glow around them. After they leave the tunnel, gradually fade the bloom down to a level where they will be able to see normally again.

For a higher-quality bloom, you can render your world using high-dynamic-range (HDR) lighting information and a floating-point back buffer format to store pixels even brighter than 1.0.

The Gaussian blur used in this sample could be replaced with any other kind of filter. For example, you could use a blur pass with large sample offsets to create various kinds of lens-flare effects.

The blur-and-combine pass is a great place to include other kinds of image manipulation. In addition to using the saturation adjustment used in this sample, you could do tone mapping here, or tint the image, or blend in a layer of noise or film grain. This infrastructure also is a good starting place for other kinds of post-processing, opening up the non-photorealistic world of edge detection, embossing, pencil sketch rendering, and other such techniques.

© 2010 Microsoft Corporation. All rights reserved.
Send feedback to xnags@microsoft.com.