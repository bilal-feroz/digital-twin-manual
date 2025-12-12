<script>
  // BLUE motion (on ORANGE)
  let blueX = 0;      // user movement amount
  const STEP = 12;

  // Perspective slope: right goes up, left goes down
  const SLOPE = 0.22;
  $: blueY = -SLOPE * blueX;

  // ✅ CALIBRATION: where BLUE should sit on ORANGE at "home"
  // You must tune these 2 numbers until it matches your Image 1.
  const BLUE_HOME_X = 340; // <-- adjust
  const BLUE_HOME_Y = 120; // <-- adjust

  // RED motion (inside BLUE)
  let redY = 0;

  // ✅ CALIBRATION: where RED sits inside BLUE at "home"
  const RED_HOME_TOP = 330;  // <-- adjust
  const RED_WIDTH = 400;     // <-- adjust

  const left = () => (blueX -= STEP);
  const right = () => (blueX += STEP);
  const up = () => (redY -= STEP);
  const down = () => (redY += STEP);

  const reset = () => {
    blueX = 0;
    redY = 0;
  };

  const BASE_WIDTH = 900;
</script>

<div class="scene">
  <div class="machine" style={`width:${BASE_WIDTH}px;`}>
    <!-- ORANGE fixed -->
    <img class="orange" src="/hori.png" alt="fixed rail" draggable="false" />

    <!-- BLUE slides on ORANGE (home offset + movement) -->
    <div
      class="blue"
      style={`transform: translate(${BLUE_HOME_X + blueX}px, ${BLUE_HOME_Y + blueY}px);`}
    >
      <img class="blue-img" src="/vetri.png" alt="vertical frame" draggable="false" />

      <!-- RED moves inside BLUE -->
      <img
        class="red"
        src="/basecon.png"
        alt="carriage"
        style={`top:${RED_HOME_TOP}px; width:${RED_WIDTH}px; transform: translate(-50%, ${redY}px);`}
        draggable="false"
      />
    </div>
  </div>

  <!-- Controls -->
  <div class="controls">
    <button on:click={up}>▲</button>
    <div class="row">
      <button on:click={left}>◀</button>
      <button class="reset" on:click={reset}>Reset</button>
      <button on:click={right}>▶</button>
    </div>
    <button on:click={down}>▼</button>
  </div>
</div>

<style>
  .scene { height: 100vh; background: #f3f3f3; overflow: hidden; position: relative; }

  .machine { position: absolute; left: 50%; top: 40px; transform: translateX(-50%); }

  .orange { width: 100%; display: block; pointer-events: none; user-select: none; }

  .blue { position: absolute; left: 0; top: 0; pointer-events: none; user-select: none; }
  .blue-img { width: 420px; display: block; } /* can be 100% if blue.png same scale; else tune */

  .red {
    position: absolute;
    left: 50%;
    pointer-events: none;
    user-select: none;
  }

  .controls {
    position: fixed; right: 20px; bottom: 20px;
    background: rgba(255,255,255,0.92);
    padding: 12px; border-radius: 12px;
    display: grid; gap: 10px; justify-items: center;
  }

  .row { display: flex; gap: 10px; align-items: center; }

  button { width: 60px; height: 60px; font-size: 22px; cursor: pointer; }
  .reset { width: 90px; font-size: 14px; }
</style>
