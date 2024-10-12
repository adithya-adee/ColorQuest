# ColorQuest

ColorQuest is a dynamic color picker application built using Svelte and HTML5 Canvas. It allows users to select colors by pointing the mouse at specific pixels in the color selection area and to adjust colors by changing the hex code. The application updates the preview in real-time as the hex code changes or the user interacts with the canvas.

## Features

- **Interactive Color Picker**: Select colors by hovering over the canvas with your mouse.
- **Hex Code Input**: Change the color of the preview div by editing the hex code.
- **Real-time Updates**: Automatically update the color across multiple formats (RGB, HSL, HSV, CMYK) when a color is selected.
- **Svelte-based**: Leverages the reactivity of Svelte for an optimized, performant color picking experience.
- **Canvas-based Rendering**: Uses the HTML5 canvas element to render the color picker, ensuring smooth interaction and rendering of the color gradients.

## Link to the Screen Recording
[![Watch the video](https://github.com/adithya-adee/ColorQuest/blob/main/video/screenRecord.mkv)

## Installation

Clone this repository to your local machine:
You can type the following command in your terminal and select the folder which you want to clone this repo. Then enter the following commands

```bash
git clone https://github.com/adithya-adee/ColorQuest.git
npm install
npm run dev
```

# Usage
Move your mouse over the canvas to select a color.
Change the hex code directly in the input field to update the preview color.
The color updates across different formats including RGB, HSL, HSV, and CMYK.
License
This project is licensed under the MIT License. See the LICENSE file for details.

# References
1. Svelte Documentation 
- https://svelte.dev/docs/introduction
- https://www.youtube.com/watch?v=zojEMeQGGHs&list=PL4cUxeGkcC9hlbrVO_2QFVqVPhlZmz7tO
2. HTML5 Canvas Documentation
  - https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas
3. Color Conversion Library
  - https://www.npmjs.com/package/color-convert  

# Acknowledgments
Special thanks to my friend for inspiring this project and helping me in Canvas.
