```js

<template>
  <div>
    <svg ref="svgElement" width="100" height="100">
      <!-- Your SVG content here -->
      <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
    </svg>
    <button @click="downloadSvgAsPng">Download as PNG</button>
  </div>
</template>

<script>
export default {
  methods: {
    downloadSvgAsPng() {
      const svgElement = this.$refs.svgElement;
      const svgString = new XMLSerializer().serializeToString(svgElement);

      const canvas = document.createElement('canvas');
      const context = canvas.getContext('2d');
      const image = new Image();

      image.onload = () => {
        canvas.width = svgElement.width.baseVal.value;
        canvas.height = svgElement.height.baseVal.value;

        context.drawImage(image, 0, 0);

        const pngDataURL = canvas.toDataURL('image/png');

        // Create a temporary link element
        const downloadLink = document.createElement('a');
        downloadLink.href = pngDataURL;
        downloadLink.download = 'converted_image.png';

        // Trigger the click event to initiate the download
        document.body.appendChild(downloadLink);
        downloadLink.click();
        document.body.removeChild(downloadLink);
      };

      image.src = 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svgString);
    },
  },
};
</script>
```

// 导出的图片清晰一点

```js
<template>
  <div>
    <svg ref="svgElement" width="100" height="100">
      <!-- Your SVG content here -->
      <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
    </svg>
    <button @click="downloadSvgAsPng">Download as PNG</button>
  </div>
</template>

<script>
export default {
  methods: {
    downloadSvgAsPng() {
      const svgElement = this.$refs.svgElement;
      const svgString = new XMLSerializer().serializeToString(svgElement);

      // Get device pixel ratio
      const pixelRatio = window.devicePixelRatio || 1;

      const canvas = document.createElement('canvas');
      const context = canvas.getContext('2d');

      // Set canvas size based on device pixel ratio
      canvas.width = svgElement.width.baseVal.value * pixelRatio;
      canvas.height = svgElement.height.baseVal.value * pixelRatio;

      // Scale context based on device pixel ratio
      context.scale(1, 1);

      const image = new Image();

      image.onload = () => {
        // Clear canvas before drawing
        context.clearRect(0, 0, canvas.width, canvas.height);

        // Draw image onto canvas
        context.drawImage(image, 0, 0, canvas.width, canvas.height);

        const pngDataURL = canvas.toDataURL('image/png');

        // Create a temporary link element
        const downloadLink = document.createElement('a');
        downloadLink.href = pngDataURL;
        downloadLink.download = 'converted_image.png';

        // Trigger the click event to initiate the download
        document.body.appendChild(downloadLink);
        downloadLink.click();
        document.body.removeChild(downloadLink);
      };

      image.src = 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svgString);
    },
  },
};
</script>

```

