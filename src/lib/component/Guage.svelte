<script lang="ts">
  import { onMount, beforeUpdate } from "svelte";

  const maxValue = 10; // Maximum value of the gauge
  let value= 80; // Initial value
  let targetValue: number; // Target value to animate to
  let svgWidth: number; // This will be set based on the element's width
  const strokeWidth = 20; // Stroke width for the gauge circle
  let radius: number; // Adjusted radius considering stroke width
  let circumference: number; // Circumference of the circle
  let dashArray: number; // Will mirror the circumference
  let dashOffset: number; // Offset for the dash stroke

  let gaugeContainer: HTMLDivElement; // Reference to the gauge container for dynamic sizing

  function initializeGauge() {
    radius = svgWidth / 2 - strokeWidth * 0.5; // Maximize radius while accommodating stroke width
    circumference = 2 * Math.PI * radius;
    dashArray = circumference;
    dashOffset = circumference - circumference * (value / maxValue);
    animateGauge();
  }

  function animateGauge() {
    const interval = setInterval(() => {
      if (value < targetValue) {
        value++;
        dashOffset = circumference - circumference * (value / maxValue);
      } else {
        clearInterval(interval);
      }
    }, 10);
  }

  onMount(() => {
    svgWidth = gaugeContainer.clientWidth; // Set SVG width based on the actual client width
    initializeGauge(); // Initialize all calculations

    window.addEventListener("resize", () => {
      svgWidth = gaugeContainer.clientWidth; // Update width on resize
      initializeGauge(); // Reinitialize gauge dimensions
    });
  });
</script>

<div bind:this={gaugeContainer} class="gauge-container">
  <svg width="100%" height="100%" viewBox={`0 0 ${svgWidth} ${svgWidth}`}>
    <defs>
      <linearGradient id="GradientColor">
        <stop offset="0%" stop-color="#00FEC9" />
        <stop offset="100%" stop-color="#008CFE" />
      </linearGradient>
    </defs>
    <circle
      cx={svgWidth / 2}
      cy={svgWidth / 2}
      r={radius}
      fill="none"
      stroke="url(#GradientColor)"
      stroke-width={strokeWidth}
      stroke-dasharray={dashArray}
      stroke-dashoffset={dashOffset}
      stroke-linecap="round"
      class="gauge-fill"
    />
  </svg>
</div>

<style>
  .gauge-container {
    width: 100%;
    max-width: 800px;
    height: auto;
    margin: auto;
  }

  svg {
    display: block;
    width: 100%;
    height: 100%;
  }

  .gauge-fill {
    transition: stroke-dashoffset 0.8s ease-out;
    transform: rotate(-220deg);
    transform-origin: center;
  }
</style>

