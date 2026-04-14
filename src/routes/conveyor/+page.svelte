<script>
	let innerDirection = null;
	let outerDirection = null;
	let selectedOuterConveyor = 'left';
	const rotation = 225;

	let innerDistance = 0;
	const maxDistance = 10;
	let holdInterval = null;
	let thirdBeltDirection = null;
	let thirdBeltLastDirection = 'left';

	const innerUpperBelt = [
		{ x: 37.4, y: 17.4 }, { x: 48.5, y: 25.7 }, { x: 57.2, y: 32.2 },
		{ x: 66.4, y: 38.2 }, { x: 75.3, y: 44.9 }, { x: 85.7, y: 52.1 },
	];
	const innerLowerBelt = [
		{ x: 17.4, y: 33.3 }, { x: 27.5, y: 40.9 }, { x: 35.8, y: 47.2 },
		{ x: 43.8, y: 52.5 }, { x: 52.1, y: 58.1 }, { x: 59.0, y: 63.5 }, { x: 66.9, y: 68.6 },
	];
	const innerThirdBelt = [
		{ x: 24.3, y: 46.4 }, { x: 31.8, y: 52.6 }, { x: 39.0, y: 58.0 }, { x: 47.2, y: 63.6 },
	];
	const thirdBeltRotation = 219;
	const outerLeftBelt = [
		{ x: 59.2, y: 82.6 }, { x: 49.7, y: 74.5 }, { x: 44.9, y: 70.3 },
		{ x: 37.1, y: 63.2 }, { x: 30.8, y: 58.3 }, { x: 19.5, y: 48.8 },
	];
	const outerRightBelt = [
		{ x: 83.6, y: 60.8 }, { x: 76.7, y: 54.2 }, { x: 70.1, y: 47.6 },
		{ x: 63.4, y: 42.4 }, { x: 56.8, y: 36.3 }, { x: 50.2, y: 30.2 }, { x: 43.9, y: 24.8 },
	];

	function getDelay(index, beltLength, direction) {
		return direction === 'outward' ? (beltLength - 1 - index) * 0.15 : index * 0.15;
	}

	function getVisibleArrowCount(totalArrows) {
		return Math.ceil((innerDistance / maxDistance) * totalArrows);
	}

	function startConveyorHold(dir) {
		thirdBeltDirection = dir;
		thirdBeltLastDirection = dir;
		if (holdInterval) clearInterval(holdInterval);
		holdInterval = setInterval(() => {
			if (dir === 'left' && innerDistance < maxDistance) {
				innerDistance = Math.min(innerDistance + 0.5, maxDistance);
			} else if (dir === 'right' && innerDistance > 0) {
				innerDistance = Math.max(innerDistance - 0.5, 0);
			}
			if ((dir === 'left' && innerDistance >= maxDistance) || (dir === 'right' && innerDistance <= 0)) {
				stopConveyorHold();
			}
		}, 100);
	}

	function stopConveyorHold() {
		if (holdInterval) { clearInterval(holdInterval); holdInterval = null; }
		thirdBeltDirection = null;
	}
</script>

<div class="page">
	<div class="grid">
		<!-- Inner Conveyor Card -->
		<div class="card">
			<div class="card-header">
				<div class="card-icon inner">
					<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2">
						<rect x="2" y="7" width="20" height="10" rx="2"/>
						<circle cx="6" cy="17" r="2"/><circle cx="18" cy="17" r="2"/>
					</svg>
				</div>
				<div class="card-title">
					<h2>Inner Conveyor</h2>
					<span class="status">{innerDirection ? innerDirection.toUpperCase() : 'IDLE'}</span>
				</div>
			</div>

			<div class="conveyor-view">
				<img src="/inner_conv.png" alt="Inner Conveyor" />
				{#if innerDirection}
					{#each innerUpperBelt as arrow, i}
						<svg class="arrow" style="top:{arrow.y}%;left:{arrow.x}%;transform:rotate({innerDirection==='outward'?rotation+180:rotation}deg);animation-delay:{innerDirection==='outward'?i*0.15:(innerUpperBelt.length-1-i)*0.15}s" viewBox="0 0 24 24" width="40" height="40">
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
					{#each innerLowerBelt as arrow, i}
						<svg class="arrow" style="top:{arrow.y}%;left:{arrow.x}%;transform:rotate({innerDirection==='outward'?rotation+180:rotation}deg);animation-delay:{innerDirection==='outward'?i*0.15:(innerLowerBelt.length-1-i)*0.15}s" viewBox="0 0 24 24" width="40" height="40">
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
				{/if}
				{#key thirdBeltLastDirection}
					{#each innerThirdBelt.slice(0, getVisibleArrowCount(innerThirdBelt.length)) as arrow, i}
						<svg class="arrow" style="top:{arrow.y}%;left:{arrow.x}%;transform:rotate({thirdBeltLastDirection==='left'?thirdBeltRotation+180:thirdBeltRotation}deg);animation-delay:{thirdBeltLastDirection==='left'?i*0.15:(innerThirdBelt.length-1-i)*0.15}s" viewBox="0 0 24 24" width="40" height="40">
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
				{/key}
			</div>

			<div class="controls">
				<button class="ctrl-btn" class:active={innerDirection==='inward'} on:pointerdown={()=>innerDirection='inward'} on:pointerup={()=>innerDirection=null} on:pointerleave={()=>innerDirection=null}>
					<svg viewBox="0 0 24 24" width="20" height="20"><path d="M19 12H5M12 19l-7-7 7-7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
					Inward
				</button>
				<button class="ctrl-btn" class:active={innerDirection==='outward'} on:pointerdown={()=>innerDirection='outward'} on:pointerup={()=>innerDirection=null} on:pointerleave={()=>innerDirection=null}>
					<svg viewBox="0 0 24 24" width="20" height="20"><path d="M5 12h14M12 5l7 7-7 7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
					Outward
				</button>
			</div>

			<div class="position-section">
				<div class="position-header">
					<span class="position-label">CONVEYOR POSITION</span>
					<span class="position-value">{innerDistance.toFixed(1)} / {maxDistance} cm</span>
				</div>
				<div class="progress-bar"><div class="progress-fill" style="width:{(innerDistance/maxDistance)*100}%"></div></div>
				<div class="position-controls">
					<button class="pos-btn" class:active={thirdBeltDirection==='right'} aria-label="Move left" on:pointerdown={()=>startConveyorHold('right')} on:pointerup={stopConveyorHold} on:pointerleave={stopConveyorHold}>
						<svg viewBox="0 0 24 24" width="18" height="18"><path d="M19 12H5M12 19l-7-7 7-7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
					</button>
					<button class="pos-btn" class:active={thirdBeltDirection==='left'} aria-label="Move right" on:pointerdown={()=>startConveyorHold('left')} on:pointerup={stopConveyorHold} on:pointerleave={stopConveyorHold}>
						<svg viewBox="0 0 24 24" width="18" height="18"><path d="M5 12h14M12 5l7 7-7 7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
					</button>
				</div>
			</div>
		</div>

		<!-- Outer Conveyor Card -->
		<div class="card">
			<div class="card-header">
				<div class="card-icon outer">
					<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2">
						<rect x="2" y="7" width="20" height="10" rx="2"/>
						<circle cx="6" cy="17" r="2"/><circle cx="18" cy="17" r="2"/>
					</svg>
				</div>
				<div class="card-title">
					<h2>Outer Conveyor</h2>
					<span class="status">{selectedOuterConveyor === 'left' ? 'OUTPUT' : 'INPUT'} SELECTED</span>
				</div>
			</div>

			<div class="toggle-group">
				<button class="toggle-btn" class:active={selectedOuterConveyor==='left'} on:click={()=>selectedOuterConveyor='left'}>OUTPUT</button>
				<button class="toggle-btn" class:active={selectedOuterConveyor==='right'} on:click={()=>selectedOuterConveyor='right'}>INPUT</button>
			</div>

			<div class="conveyor-view">
				<img src="/outconv.png" alt="Outer Conveyor" />
				{#if outerDirection}
					{#each outerLeftBelt as arrow, i}
						<svg class="arrow yellow" style="top:{arrow.y}%;left:{arrow.x}%;transform:rotate({outerDirection==='outward'?rotation+180:rotation}deg);animation-delay:{getDelay(i,outerLeftBelt.length,outerDirection)}s" viewBox="0 0 24 24" width="40" height="40">
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="#f59e0b" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
					{#each outerRightBelt as arrow, i}
						<svg class="arrow yellow" style="top:{arrow.y}%;left:{arrow.x}%;transform:rotate({outerDirection==='outward'?rotation+180:rotation}deg);animation-delay:{getDelay(i,outerRightBelt.length,outerDirection)}s" viewBox="0 0 24 24" width="40" height="40">
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="#f59e0b" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
				{/if}
			</div>

			<div class="controls">
				<button class="ctrl-btn" class:active={outerDirection==='inward'} on:pointerdown={()=>outerDirection='inward'} on:pointerup={()=>outerDirection=null} on:pointerleave={()=>outerDirection=null}>
					<svg viewBox="0 0 24 24" width="20" height="20"><path d="M19 12H5M12 19l-7-7 7-7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
					Inward
				</button>
				<button class="ctrl-btn" class:active={outerDirection==='outward'} on:pointerdown={()=>outerDirection='outward'} on:pointerup={()=>outerDirection=null} on:pointerleave={()=>outerDirection=null}>
					<svg viewBox="0 0 24 24" width="20" height="20"><path d="M5 12h14M12 5l7 7-7 7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
					Outward
				</button>
			</div>
		</div>
	</div>
</div>

<style>
	.page {
		padding: 24px;
		padding-top: 70px;
		min-height: 100vh;
		box-sizing: border-box;
		background: #f5f5f5;
	}

	.grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 24px;
		height: calc(100vh - 94px);
	}

	@media (max-width: 1200px) {
		.grid {
			grid-template-columns: 1fr;
			height: auto;
		}
	}

	.card {
		background: #fff;
		border-radius: 16px;
		padding: 24px;
		box-shadow: 0 2px 8px rgba(0,0,0,0.06);
		display: flex;
		flex-direction: column;
	}

	.card-header {
		display: flex;
		align-items: center;
		gap: 16px;
		margin-bottom: 20px;
	}

	.card-icon {
		width: 48px;
		height: 48px;
		border-radius: 12px;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
	}

	.card-icon.inner {
		background: #dbeafe;
		color: #2563eb;
	}

	.card-icon.outer {
		background: #fef3c7;
		color: #d97706;
	}

	.card-title h2 {
		margin: 0;
		font-size: 20px;
		font-weight: 600;
		color: #1a1a1a;
	}

	.status {
		font-size: 12px;
		color: #888;
		text-transform: uppercase;
		letter-spacing: 0.5px;
	}

	.conveyor-view {
		position: relative;
		background: #f8f9fa;
		border-radius: 12px;
		border: 1px solid #e5e7eb;
		padding: 20px;
		margin-bottom: 20px;
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.conveyor-view img {
		max-width: 100%;
		max-height: 100%;
		height: auto;
		display: block;
	}

	.arrow {
		position: absolute;
		translate: -50% -50%;
		animation: chase 2s infinite;
		filter: drop-shadow(1px 1px 2px rgba(0,0,0,0.5));
	}

	.arrow.yellow {
		animation: chase-yellow 2s infinite;
	}

	@keyframes chase {
		0%, 100% { opacity: 0.2; }
		20%, 30% { opacity: 1; filter: drop-shadow(0 0 8px rgba(255,0,0,0.9)); }
	}

	@keyframes chase-yellow {
		0%, 100% { opacity: 0.2; }
		20%, 30% { opacity: 1; filter: drop-shadow(0 0 8px rgba(245,158,11,0.9)); }
	}

	.controls {
		display: flex;
		gap: 12px;
	}

	.ctrl-btn {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 10px;
		padding: 16px 24px;
		background: #f3f4f6;
		border: 2px solid transparent;
		border-radius: 12px;
		font-size: 15px;
		font-weight: 600;
		color: #4b5563;
		cursor: pointer;
		transition: all 0.15s ease;
	}

	.ctrl-btn:hover {
		background: #e5e7eb;
	}

	.ctrl-btn.active {
		background: #2563eb;
		color: #fff;
		box-shadow: 0 4px 12px rgba(37, 99, 235, 0.3);
	}

	.toggle-group {
		display: flex;
		gap: 8px;
		margin-bottom: 16px;
	}

	.toggle-btn {
		flex: 1;
		padding: 12px 20px;
		border: 2px solid #e5e7eb;
		background: #fff;
		border-radius: 10px;
		font-size: 13px;
		font-weight: 700;
		color: #6b7280;
		cursor: pointer;
		transition: all 0.15s ease;
		letter-spacing: 0.5px;
	}

	.toggle-btn:hover {
		border-color: #d1d5db;
		background: #f9fafb;
	}

	.toggle-btn.active {
		background: #1e3a5f;
		border-color: #1e3a5f;
		color: #fff;
	}

	.position-section {
		margin-top: 24px;
		padding-top: 20px;
		border-top: 1px solid #e5e7eb;
	}

	.position-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 12px;
	}

	.position-label {
		font-size: 12px;
		font-weight: 700;
		color: #9ca3af;
		letter-spacing: 0.5px;
	}

	.position-value {
		font-size: 16px;
		font-weight: 700;
		color: #1a1a1a;
	}

	.progress-bar {
		height: 10px;
		background: #e5e7eb;
		border-radius: 5px;
		overflow: hidden;
		margin-bottom: 16px;
	}

	.progress-fill {
		height: 100%;
		background: linear-gradient(90deg, #22c55e, #16a34a);
		border-radius: 5px;
		transition: width 0.1s ease;
	}

	.position-controls {
		display: flex;
		justify-content: center;
		gap: 12px;
	}

	.pos-btn {
		width: 48px;
		height: 48px;
		border: 2px solid #e5e7eb;
		background: #fff;
		border-radius: 10px;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		color: #4b5563;
		transition: all 0.15s ease;
	}

	.pos-btn:hover {
		background: #f3f4f6;
		border-color: #d1d5db;
	}

	.pos-btn.active {
		background: #2563eb;
		border-color: #2563eb;
		color: #fff;
	}
</style>
