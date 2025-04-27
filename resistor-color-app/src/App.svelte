<script>
  import { fade } from 'svelte/transition';
  // loading animation
  let loading = true;
  
  setTimeout(() => {
    loading = false;
  }, 1500); // 1.5 seconds
  
  const colors = [
    "black", "brown", "red", "orange", "yellow",
    "green", "blue", "violet", "gray", "white",
    "gold", "silver"
  ];
  
  const colorCodes = {
    black:  { digit: 0, multiplier: 1 },
    brown:  { digit: 1, multiplier: 10, tolerance: 1 },
    red:    { digit: 2, multiplier: 100, tolerance: 2 },
    orange: { digit: 3, multiplier: 1000 },
    yellow: { digit: 4, multiplier: 10000 },
    green:  { digit: 5, multiplier: 100000, tolerance: 0.5 },
    blue:   { digit: 6, multiplier: 1000000, tolerance: 0.25 },
    violet: { digit: 7, multiplier: 10000000, tolerance: 0.1 },
    gray:   { digit: 8, multiplier: 100000000, tolerance: 0.05 },
    white:  { digit: 9, multiplier: 1000000000 },
    gold:   { multiplier: 0.1, tolerance: 5 },
    silver: { multiplier: 0.01, tolerance: 10 },
  };
  
  const colorMap = {
    black: "#000",
    brown: "#8B4513",
    red: "#FF0000",
    orange: "#FFA500",
    yellow: "#FFFF00",
    green: "#008000",
    blue: "#0000FF",
    violet: "#8A2BE2",
    gray: "#808080",
    white: "#FFFFFF",
    gold: "#FFD700",
    silver: "#C0C0C0"
  };
  
  let band1 = "red";
  let band2 = "violet";
  let band3 = "yellow";
  let band4 = "gold";
  let result = "";
  
  function decode() {
    const digit1 = colorCodes[band1]?.digit;
    const digit2 = colorCodes[band2]?.digit;
    const multiplier = colorCodes[band3]?.multiplier;
    const tolerance = colorCodes[band4]?.tolerance ?? 20;
    
    if (digit1 === undefined || digit2 === undefined || multiplier === undefined) {
      result = "Invalid selection.";
      return;
    }
    
    const resistance = (digit1 * 10 + digit2) * multiplier;
    result = `${resistance.toLocaleString()} Ω ±${tolerance}%`;
  }
  
</script>

{#if loading}
  <div class="load-container" transition:fade>
    <div class="spinner"></div>
  </div>

{:else}
  <h1>🎨 Resistor Color Code Decoder</h1>

  <div class="bands">
    <select bind:value={band1}>{#each colors as c}<option>{c}</option>{/each}</select>
    <select bind:value={band2}>{#each colors as c}<option>{c}</option>{/each}</select>
    <select bind:value={band3}>{#each colors as c}<option>{c}</option>{/each}</select>
    <select bind:value={band4}>{#each colors as c}<option>{c}</option>{/each}</select>
  </div>

  <!-- Visual Resistor -->
  <div class="resistor">
    <div class="resistor-body">
      <div class="band" style="left: 20%;" style:background={colorMap[band1]}></div>
      <div class="band" style="left: 35%;" style:background={colorMap[band2]}></div>
      <div class="band" style="left: 50%;" style:background={colorMap[band3]}></div>
      <div class="band" style="left: 65%;" style:background={colorMap[band4]}></div>
    </div>
  </div>

  <button on:click={decode}>Decode</button>

  {#if result}
  <p class="result">{result}</p>
  {/if}

{/if}
