<script>
	import favicon from '$lib/assets/favicon.svg';
	import { page } from '$app/state';

	let { children } = $props();

	const links = [
		{ href: '/current', label: 'Current' },
		{ href: '/conveyor', label: 'Conveyor' }
	];
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<nav class="topnav">
	{#each links as link}
		<a href={link.href} class:active={page.url.pathname === link.href}>
			{link.label}
		</a>
	{/each}
</nav>

<main class="page">
	{@render children()}
</main>

<style>
	.topnav {
		position: sticky;
		top: 0;
		z-index: 10;
		display: flex;
		gap: 12px;
		padding: 12px 16px;
		background: white;
		border-bottom: 1px solid #e6e6e6;
	}

	.topnav a {
		text-decoration: none;
		color: #333;
		font-weight: 600;
		padding: 6px 10px;
		border-radius: 8px;
	}

	.topnav a.active {
		background: #111827;
		color: white;
	}

	.page {
		min-height: calc(100vh - 52px);
	}
</style>
