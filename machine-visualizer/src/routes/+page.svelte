<script>
  // Gantry position state
  let gantryX = $state(0);
  let gantryY = $state(0);

  // Belt direction: 1 = forward (right), -1 = backward (left)
  let beltDirection = $state(1);

  // Movement step in pixels
  const step = 15;

  // Bounds for gantry movement
  const bounds = {
    minX: -200,
    maxX: 200,
    minY: -150,
    maxY: 150
  };

  function handleKey(event) {
    switch (event.key) {
      case 'ArrowLeft':
        gantryX = Math.max(bounds.minX, gantryX - step);
        beltDirection = -1;
        event.preventDefault();
        break;
      case 'ArrowRight':
        gantryX = Math.min(bounds.maxX, gantryX + step);
        beltDirection = 1;
        event.preventDefault();
        break;
      case 'ArrowUp':
        gantryY = Math.max(bounds.minY, gantryY - step);
        event.preventDefault();
        break;
      case 'ArrowDown':
        gantryY = Math.min(bounds.maxY, gantryY + step);
        event.preventDefault();
        break;
    }
  }
</script>

<svelte:window onkeydown={handleKey} />

<div class="scene">
  <!-- Conveyor Belt -->
  <div class="conveyor-container">
    <img src="/conveyor.png" alt="Conveyor Belt" class="conveyor-image" />
    <!-- Animated belt overlay -->
    <div
      class="belt-overlay"
      class:reverse={beltDirection === -1}
    ></div>
  </div>

  <!-- Gantry System (moves together) -->
  <div
    class="gantry-system"
    style="transform: translateX({gantryX}px) translateY({gantryY}px)"
  >
    <!-- Left vertical column -->
    <img
      src="/gantry-vertical.png"
      alt="Left Column"
      class="gantry-left"
    />

    <!-- Horizontal rail -->
    <img
      src="/gantry-horizontal.png"
      alt="Horizontal Gantry Rail"
      class="gantry-horizontal"
    />

    <!-- Right vertical column (mirrored) -->
    <img
      src="/gantry-vertical.png"
      alt="Right Column"
      class="gantry-right"
    />
  </div>

  <!-- Controls Info -->
  <div class="controls-info">
    <h3>Keyboard Controls</h3>
    <p><kbd>Arrow Left</kbd> / <kbd>Arrow Right</kbd> - Move gantry X + change belt direction</p>
    <p><kbd>Arrow Up</kbd> / <kbd>Arrow Down</kbd> - Move gantry Y</p>
    <div class="status">
      <span>Gantry X: {gantryX}px</span>
      <span>Gantry Y: {gantryY}px</span>
      <span>Belt: {beltDirection === 1 ? 'Forward →' : '← Reverse'}</span>
    </div>
  </div>
</div>

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :global(body) {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
    min-height: 100vh;
    overflow: hidden;
  }

  .scene {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    perspective: 1000px;
  }

  /* Conveyor Belt */
  .conveyor-container {
    position: absolute;
    width: 500px;
    height: 350px;
    z-index: 1;
  }

  .conveyor-image {
    width: 100%;
    height: 100%;
    object-fit: contain;
    filter: drop-shadow(0 10px 30px rgba(0, 0, 0, 0.5));
  }

  /* Animated belt overlay - simulates movement */
  .belt-overlay {
    position: absolute;
    top: 42%;
    left: 10%;
    width: 80%;
    height: 16%;
    background: repeating-linear-gradient(
      90deg,
      transparent 0px,
      transparent 20px,
      rgba(0, 0, 0, 0.15) 20px,
      rgba(0, 0, 0, 0.15) 40px
    );
    animation: beltMove 0.5s linear infinite;
    pointer-events: none;
    border-radius: 2px;
  }

  .belt-overlay.reverse {
    animation-direction: reverse;
  }

  @keyframes beltMove {
    0% {
      background-position: 0 0;
    }
    100% {
      background-position: 40px 0;
    }
  }

  /* Gantry System */
  .gantry-system {
    position: absolute;
    z-index: 10;
    transition: transform 0.1s ease-out;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .gantry-horizontal {
    width: 300px;
    height: auto;
    object-fit: contain;
    filter: drop-shadow(0 5px 20px rgba(0, 0, 0, 0.4));
    position: relative;
    z-index: 3;
  }

  .gantry-left {
    position: absolute;
    left: -120px;
    top: 50px;
    width: 120px;
    height: auto;
    object-fit: contain;
    filter: drop-shadow(-5px 5px 15px rgba(0, 0, 0, 0.4));
    z-index: 2;
  }

  .gantry-right {
    position: absolute;
    right: -120px;
    top: 50px;
    width: 120px;
    height: auto;
    object-fit: contain;
    filter: drop-shadow(5px 5px 15px rgba(0, 0, 0, 0.4));
    transform: scaleX(-1); /* Mirror the image */
    z-index: 2;
  }

  /* Controls Info Panel */
  .controls-info {
    position: fixed;
    bottom: 20px;
    left: 20px;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    padding: 20px;
    border-radius: 12px;
    color: white;
    font-family: 'Segoe UI', system-ui, sans-serif;
    border: 1px solid rgba(255, 255, 255, 0.2);
    z-index: 100;
  }

  .controls-info h3 {
    margin-bottom: 10px;
    font-size: 14px;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: #64ffda;
  }

  .controls-info p {
    font-size: 12px;
    margin: 5px 0;
    opacity: 0.9;
  }

  .controls-info kbd {
    background: rgba(255, 255, 255, 0.2);
    padding: 2px 8px;
    border-radius: 4px;
    font-family: monospace;
    font-size: 11px;
    border: 1px solid rgba(255, 255, 255, 0.3);
  }

  .status {
    margin-top: 15px;
    padding-top: 10px;
    border-top: 1px solid rgba(255, 255, 255, 0.2);
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .status span {
    font-family: monospace;
    font-size: 12px;
    color: #f0f0f0;
  }
</style>
