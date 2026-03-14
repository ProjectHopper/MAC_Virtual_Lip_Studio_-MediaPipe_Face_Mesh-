# M·A·C Virtual Lip Studio
A real-time, browser-based AR lipstick try-on experience inspired by M·A·C Cosmetics. Uses your webcam and Google's MediaPipe Face Mesh to track your lips and apply virtual lipstick color — no app install, no data upload, everything runs locally on your device.


![image alt](https://github.com/ProjectHopper/MAC_Virtual_Lip_Studio_-MediaPipe_Face_Mesh-/blob/e904e3aedbca3c13465ca0bf362123643a75545d/Screenshot%202026-03-14%20130020.jpg)


![image alt](https://github.com/ProjectHopper/MAC_Virtual_Lip_Studio_-MediaPipe_Face_Mesh-/blob/601d807d09be3e0bec39a1716475ec734217280c/Screenshot%202026-03-14%20130119.png)


![image alt](https://github.com/ProjectHopper/MAC_Virtual_Lip_Studio_-MediaPipe_Face_Mesh-/blob/601d807d09be3e0bec39a1716475ec734217280c/Screenshot%202026-03-14%20184943.png)

## Demo
Open:

    mac-virtual-lip-studio.html
    
directly in your browser — no server required.

## Features

- **Real-time lip tracking** : using MediaPipe Face Mesh (468 facial landmarks)
- **Virtual lipstick overlay** : with gloss highlight and shadow rendering
- **Head-turn gesture control** : turn right for next shade, turn left for previous
- **Personal baseline calibration** : adapts to your neutral head position automatically
- **12 MAC-inspired shades** : spanning nudes, reds, berries, and darks
- **Tap swatches** : to jump directly to any color
- **Keyboard support** : arrow keys to cycle shades
- **Zero data collection** : all processing happens in your browser, nothing is sent anywhere
- **No install, no dependencies** : single .html file, loads libraries from CDN


## How to Use

1. Download

    mac-virtual-lip-studio.html

    
2. Open it in Chrome or Edge (recommended for best camera API support)

3. Click Begin Try-On and allow camera access when prompted

4. Hold still for ~1 second while the app calibrates your neutral head position
5. Try on shades:

### Action           Result


Turn head right   ->   Next shade


Turn head left    ->   Previous shade


Click a swatch    ->   Jump to that shade


Arrow keys ← →       Cycle shades


## Tips for Best Results

Use front-facing lighting — light should be on your face, not behind you


Sit about arm's length from the camera


During the calibration phase, look straight at the camera


Keep your mouth visible — avoid covering your lips


## How It Works
Face Tracking



MediaPipe Face Mesh detects 468 3D landmarks on the face in real time via the browser's camera API. The app uses a subset of these landmarks:

20 outer lip points + 20 inner lip points — for tracing and rendering the lip shape



Nose tip (landmark 1) and cheekbone anchors (landmarks 234, 454) — for calculating horizontal head rotation (yaw)

## Head Turn Detection
Yaw is estimated by comparing the nose tip X position relative to the midpoint between the two cheekbones. Rather than using an absolute threshold (which would cause false triggers on asymmetric faces), the app:

Calibrates a personal baseline over the first 20 frames
Computes delta from baseline each frame
Triggers a shade change only when delta exceeds ±0.16
Uses a direction lock so the same direction can't re-trigger until the head returns to center

## Lip Rendering
Lip color is drawn on an HTML <canvas> element overlaid on the video feed (both mirrored with scaleX(-1)):

Outer lip contour drawn as smooth quadratic bezier curves
Inner mouth area cut out using destination-out compositing (so teeth stay visible)
A radial gradient adds a gloss highlight on the center of the lips
A soft drop shadow reinforces the 3D appearance


# Tech Stack
MediaPipe Face Mesh: Real-time facial landmark detection


MediaPipe Camera Utils- Camera frame capture loop


HTML5 Canvas API - Lip color rendering and compositing


getUserMedia API - Webcam access


Vanilla JavaScript - All application logic


Google Fonts (Cormorant Garamond + Montserrat) - Typography
No frameworks. No build step. One file.


## Privacy
This app runs entirely in your browser. Your camera feed is:

Never uploaded to any server


Never stored or recorded


Only processed locally by MediaPipe's WebAssembly model

The only external network requests are to load the MediaPipe library and font files from CDN on first load.

## Customizing Shades
Shades are defined in the SHADES array near the top of the script. Each shade has this shape:
    
    js{
      name:    'Ruby Woo',
      hex:     '#c0032f',   // swatch color
      r: 192, g: 3, b: 47, // RGB for canvas rendering
      finish:  'Retro Matte',
      opacity: 0.72         // 0 = invisible, 1 = fully opaque
    }




Add, remove, or edit entries to customize the shade collection.

## License
MIT — free to use, modify, and distribute.

## Disclaimer
This project is an independent fan-made demo. It is not affiliated with, endorsed by, or connected to M·A·C Cosmetics or Estée Lauder Companies. All shade names are used for reference purposes only.
