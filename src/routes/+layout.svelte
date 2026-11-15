<script lang="ts">
  import DemoLinks from './demo/DemoLinks.svelte';
  import type { Path } from '$app/types';
  import { resolve } from '$app/paths';
  import { page } from '$app/state';
  import { locales, localizeHref, setLocale } from '#lib/paraglide/runtime.js';
  import favicon from '#lib/assets/favicon.png';
  import type { LayoutProps } from './$types';

  let { children }: LayoutProps = $props();

  let isDark = $state(true);
  let theme = $derived(isDark ? 'dark' : 'light');

  $effect(() => {
    document.body.classList.toggle('dark', isDark);
  });

  function toggleThemeMode() {
    isDark = !isDark;
  }
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

	<button onclick={() => setLocale('en')}>en</button>  <!-- language -->

	<!-- <button onclick={() => setLocale('de')}>de</button>  darkmode -->
	 
	<button
		onclick={toggleThemeMode}>
		{ theme === 'dark' ? '☀️' : '🌙'}
	</button>  <!-- darkmode -->
</nav>

{@render children()}

<div style="display:none">
	{#each locales as locale (locale)}
		<a href={resolve(localizeHref(page.url.pathname, { locale }) as Path)}>{locale}</a>
	{/each}
</div>

<DemoLinks />
