<script>
  // ===== FILES =====
  const ORANGE_SRC = "/hhori.png";
  const BLUE_SRC = "/hverti.png";
  const RED_SRC = "/hbasecon.png";

  // ===== HARD-CODED LAYOUT (your calibrated positions) =====
  const orange = { x: 442, y: 14, w: 857 };
  const blueHome = { x: 722, y: 31, w: 315 };
  const redHome = { x: 40, y: 246, w: 231 };

  // ===== FINAL LIMITS (your measured values) =====
  const BLUE_MIN = -222;
  const BLUE_MAX = 132;

  const RED_MIN = -144;
  const RED_MAX = 282;

  // ===== MOTION STATE =====
  let blueX = 0;
  let redY = 0;

  // ===== HOLD CONTROLS =====
  const STEP = 6;
  const TICK = 25;
  let timer = null;

  const clamp = (v, min, max) => Math.max(min, Math.min(max, v));

  function move(dx, dy) {
    blueX = clamp(blueX + dx, BLUE_MIN, BLUE_MAX);
    redY = clamp(redY + dy, RED_MIN, RED_MAX);
  }

  function start(dx, dy) {
    stop();
    move(dx, dy); // immediate step
    timer = setInterval(() => move(dx, dy), TICK);
  }

  function stop() {
    if (timer) clearInterval(timer);
    timer = null;
  }

  function reset() {
    blueX = 0;
    redY = 0;
  }

  // Keyboard arrows (tap-to-step)
  function onKeyDown(e) {
    if (e.key === "ArrowLeft") move(-STEP, 0);
    if (e.key === "ArrowRight") move(STEP, 0);
    if (e.key === "ArrowUp") move(0, -STEP);
    if (e.key === "ArrowDown") move(0, STEP);
  }
</script>

<svelte:window on:keydown={onKeyDown} />

<div class="scene">
  <div class="machine">
    <!-- ORANGE fixed -->
    <img
      class="orange"
      src={ORANGE_SRC}
      alt="orange"
      style={`left:${orange.x}px; top:${orange.y}px; width:${orange.w}px;`}
      draggable="false"
    />

    <!-- BLUE moves left/right from HOME -->
    <div
      class="blue"
      style={`left:${blueHome.x}px; top:${blueHome.y}px; width:${blueHome.w}px; transform: translateX(${blueX}px);`}
    >
      <img class="blueImg" src={BLUE_SRC} alt="blue" draggable="false" />

      <!-- RED moves up/down from HOME inside BLUE -->
      <img
        class="red"
        src={RED_SRC}
        alt="red"
        style={`left:${redHome.x}px; top:${redHome.y}px; width:${redHome.w}px; transform: translateY(${redY}px);`}
        draggable="false"
      />
    </div>
  </div>

  <!-- Controls -->
  <div class="controls">
    <button
      aria-label="Up"
      on:mousedown={() => start(0, -STEP)}
      on:mouseup={stop}
      on:mouseleave={stop}
      on:touchstart|preventDefault={() => start(0, -STEP)}
      on:touchend={stop}
    >↑</button>

    <div class="row">
      <button
        aria-label="Left"
        on:mousedown={() => start(-STEP, 0)}
        on:mouseup={stop}
        on:mouseleave={stop}
        on:touchstart|preventDefault={() => start(-STEP, 0)}
        on:touchend={stop}
      >←</button>

      <button class="reset" aria-label="Reset" on:click={reset}>Reset</button>

      <button
        aria-label="Right"
        on:mousedown={() => start(STEP, 0)}
        on:mouseup={stop}
        on:mouseleave={stop}
        on:touchstart|preventDefault={() => start(STEP, 0)}
        on:touchend={stop}
      >→</button>
    </div>

    <button
      aria-label="Down"
      on:mousedown={() => start(0, STEP)}
      on:mouseup={stop}
      on:mouseleave={stop}
      on:touchstart|preventDefault={() => start(0, STEP)}
      on:touchend={stop}
    >↓</button>
  </div>
</div>

<style>
  .scene {
    height: 100vh;
    background: #eee;
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .machine {
    position: relative;
    width: 1300px;
    height: 600px;
  }

  .orange {
    position: absolute;
    pointer-events: none;
    user-select: none;
  }

  .blue {
    position: absolute;
    pointer-events: none;
    user-select: none;
  }

  .blueImg {
    width: 100%;
    display: block;
  }

  .red {
    position: absolute;
    pointer-events: none;
    user-select: none;
  }

  /* ===== Modern Controls (bottom-left) ===== */
  .controls {
    position: fixed;
    left: 18px;
    bottom: 18px;

    display: grid;
    gap: 10px;
    justify-items: center;

    padding: 14px;
    border-radius: 18px;

    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);

    box-shadow:
      0 12px 30px rgba(0, 0, 0, 0.12),
      0 2px 8px rgba(0, 0, 0, 0.08);

    border: 1px solid rgba(255, 255, 255, 0.6);
    z-index: 1000;
  }

  .row {
    display: flex;
    gap: 10px;
    align-items: center;
  }

  button {
    width: 56px;
    height: 56px;
    border-radius: 16px;

    border: 1px solid rgba(0, 0, 0, 0.08);
    background: rgba(255, 255, 255, 0.9);

    font-size: 22px;
    cursor: pointer;

    box-shadow:
      0 8px 18px rgba(0, 0, 0, 0.10),
      inset 0 1px 0 rgba(255, 255, 255, 0.7);

    transition: transform 120ms ease, box-shadow 120ms ease, background 120ms ease;
    touch-action: manipulation;
  }

  button:hover {
    transform: translateY(-1px);
    box-shadow:
      0 10px 22px rgba(0, 0, 0, 0.12),
      inset 0 1px 0 rgba(255, 255, 255, 0.75);
  }

  button:active {
    transform: translateY(1px) scale(0.98);
    box-shadow:
      0 6px 14px rgba(0, 0, 0, 0.10),
      inset 0 2px 6px rgba(0, 0, 0, 0.08);
  }

  .reset {
    width: 96px;
    height: 56px;
    border-radius: 16px;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.2px;
  }
</style>