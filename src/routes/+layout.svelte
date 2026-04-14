<script>
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<nav class="nav-tabs">
	<a href="/current">Arm</a>
	<a href="/conveyor">Conveyor</a>
</nav>

{@render children()}

<style>
	.nav-tabs {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 9999;
		display: flex;
		gap: 0;
		background: #1a1a1a;
		padding: 0 16px;
		border-bottom: 1px solid #333;
	}

	.nav-tabs a {
		padding: 14px 24px;
		color: #888;
		text-decoration: none;
		font-weight: 500;
		font-size: 14px;
		border-bottom: 2px solid transparent;
		transition: all 0.2s ease;
	}

	.nav-tabs a:hover {
		color: #bbb;
		background: rgba(255, 255, 255, 0.05);
	}

	.nav-tabs a.active {
		color: #fff;
		border-bottom-color: #3b82f6;
	}
</style>