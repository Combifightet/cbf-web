<script lang="ts">
	import favicon from '#lib/assets/favicon.png';
	import type { LayoutProps } from './$types';

	let { children }: LayoutProps = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<nav>
	<a href="/">home</a>
	<a href="/art">art</a>
	<a href="/projects">projects</a>
	<a href="/quotes">quotes</a>
	<a href="/recepies">recepies</a>
</nav>

{@render children()}

