<script>
  import { onMount } from "svelte";
  import convert from "color-convert"; // Assuming color-convert is properly installed

  let canvas, preview, huePicker;
  let pickedHue = 0;
  let rgb = [255, 0, 0]; // Initial color red
  let hex, hsl, hsv, cmyk;

  // Updates all color formats when the RGB color changes
  $: updateFormats(rgb);

  // Ensures the canvas is properly initialized when mounted
  onMount(() => {
    const box = canvas.getBoundingClientRect();
    canvas.width = box.width;
    initCanvas(canvas);
  });

  // Initializes the canvas and color picker rendering
  function initCanvas(canvas) {
    const ctx = canvas.getContext("2d");
    setColorPicker(canvas, `hsl(${pickedHue}, 100%, 50%)`);

    canvas.addEventListener("mousemove", (e) => {
      const box = canvas.getBoundingClientRect();
      const x = Math.floor(e.clientX - box.left);
      const y = Math.floor(e.clientY - box.top);

      // Get color data from the canvas at the current mouse position
      const { data } = ctx.getImageData(x, y, 1, 1);
      rgb = [data[0], data[1], data[2]];

      // Update preview background with the selected color
      preview.style.background = `rgb(${rgb[0]}, ${rgb[1]}, ${rgb[2]})`;
    });
  }

  // Draws the color picker on the canvas based on the current hue
  function setColorPicker(canvas, color) {
    const ctx = canvas.getContext("2d");

    ctx.fillStyle = color;
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Apply saturation gradient
    const saturationGradient = ctx.createLinearGradient(0, 0, canvas.width, 0);
    saturationGradient.addColorStop(0, "white");
    saturationGradient.addColorStop(1, "transparent");
    ctx.fillStyle = saturationGradient;
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Apply lightness gradient
    const lightnessGradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
    lightnessGradient.addColorStop(0, "transparent");
    lightnessGradient.addColorStop(1, "black");
    ctx.fillStyle = lightnessGradient;
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  }

  // Updates color formats based on the current RGB values
  function updateFormats([r, g, b]) {
    hex = convert.rgb.hex(r, g, b);
    hsl = convert.rgb.hsl(r, g, b);
    hsv = convert.rgb.hsv(r, g, b);
    cmyk = convert.rgb.cmyk(r, g, b);
  }

  // Functions to update RGB color from different input formats
  function updateColorFromHex(value) {
    try {
      rgb = convert.hex.rgb(value.replace("#", ""));
    } catch {
      // Ignore invalid input
    }
  }

  function updateColorFromRgb(value) {
    const [r, g, b] = value.split(",").map(Number);
    if (!isNaN(r) && !isNaN(g) && !isNaN(b)) {
      rgb = [r, g, b];
    }
  }

  function updateColorFromHsl(value) {
    const [h, s, l] = value.split(",").map(Number);
    if (!isNaN(h) && !isNaN(s) && !isNaN(l)) {
      rgb = convert.hsl.rgb(h, s, l);
    }
  }

  function updateColorFromCmyk(value) {
    const [c, m, y, k] = value.split(",").map(Number);
    if (!isNaN(c) && !isNaN(m) && !isNaN(y) && !isNaN(k)) {
      rgb = convert.cmyk.rgb(c, m, y, k);
    }
  }

  function updateColorFromHsv(value) {
    const [h, s, v] = value.split(",").map(Number);
    if (!isNaN(h) && !isNaN(s) && !isNaN(v)) {
      rgb = convert.hsv.rgb(h, s, v);
    }
  }
</script>

<!-- Main Component -->
<div class="main-Wrapper">
  <h1 class="head">Welcome To My Color Picker</h1>
  <div class="wrapper">
    <canvas class="cnv" bind:this={canvas}></canvas>
    <div class="preview" bind:this={preview}></div>

    <!-- HEX Format -->
    <span class="color-span">HEX</span>
    <input
      type="text"
      value={`#${hex}`}
      on:input={(e) => updateColorFromHex(e.target.value)}
    />

    <!-- RGB Format -->
    <div class="container">
      <div class="color-div">
        <span class="color-span">RGB</span>
        <input
          class="color-format"
          value={rgb.join(",")}
          on:input={(e) => updateColorFromRgb(e.target.value)}
        />
      </div>

      <!-- CMYK Format -->
      <div class="color-div">
        <span class="color-span">CMYK</span>
        <input
          class="color-format"
          value={cmyk.join(",")}
          on:input={(e) => updateColorFromCmyk(e.target.value)}
        />
      </div>

      <!-- HSV Format -->
      <div class="color-div">
        <span class="color-span">HSV</span>
        <input
          class="color-format"
          value={hsv.join(",")}
          on:input={(e) => updateColorFromHsv(e.target.value)}
        />
      </div>

      <!-- HSL Format -->
      <div class="color-div">
        <span class="color-span">HSL</span>
        <input
          class="color-format"
          value={hsl.join(",")}
          on:input={(e) => updateColorFromHsl(e.target.value)}
        />
      </div>
    </div>

    <!-- Hue Picker -->
    <input
      type="range"
      bind:this={huePicker}
      min="0"
      max="360"
      bind:value={pickedHue}
      on:input={() => setColorPicker(canvas, `hsl(${pickedHue}, 100%, 50%)`)}
    />
  </div>
</div>

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .main-Wrapper {
    width: 220vh;
    height: 100vh;
  }

  .wrapper {
    border: 1px solid black;
    display: flex;
    flex-direction: column;
    width: 80vw;
    padding: 10px;
    gap: 1rem;
    border-radius: 0.5rem;
    background: white;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 1rem rgba(0, 0, 0, 0.6);
  }

  .cnv {
    border: 1px solid black;
  }

  .preview {
    height: 100px;
    border: 1px solid black;
  }

  .container {
    display: flex;
    justify-content: space-evenly;
  }

  .color-div {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .color-span {
    margin-bottom: 0.5rem;
  }

  input.color-format {
    border: 2px solid red;
    border-radius: 10rem;
    padding: 0.2rem;
  }

  .head {
    display: flex;
    justify-content: center;
    padding: 10px;
  }
</style>
