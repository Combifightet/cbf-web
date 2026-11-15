<script lang="ts">
  import DemoLinks from './demo/DemoLinks.svelte';
  import type { Path } from '$app/types';
  import { resolve } from '$app/paths';
  import { page } from '$app/state';
  import { locales, localizeHref, setLocale } from '#lib/paraglide/runtime.js';
  import favicon from '#lib/assets/favicon.png';
  import type { LayoutProps } from './$types';

  // Import each category's stylesheet as a URL
  import homeTheme from '#lib/themes/home.css?url';
  import artTheme from '#lib/themes/art.css?url';
  import projectsTheme from '#lib/themes/projects.css?url';
  import quotesTheme from '#lib/themes/quotes.css?url';
  import recepiesTheme from '#lib/themes/recepies.css?url';

  let { children }: LayoutProps = $props();

  let isDark = $state(true);

  // Pick the stylesheet URL for the current route
  let activeThemeUrl = $derived.by(() => {
    const route = page.route.id ?? '/';
    if (route.startsWith('/art')) return artTheme;
    if (route.startsWith('/projects')) return projectsTheme;
    if (route.startsWith('/quotes')) return quotesTheme;
    if (route.startsWith('/recepies')) return recepiesTheme;
    return homeTheme;
  });

  // Toggle dark mode on <body>
  $effect(() => {
    document.body.classList.toggle('dark', isDark);
  });

  function toggleThemeMode() {
    isDark = !isDark;
  }
</script>

<svelte:head>
  <link rel="icon" href={favicon} />
  <link rel="stylesheet" href={activeThemeUrl} />
</svelte:head>

<nav>
  <a href="/">home</a>
  <a href="/art">art</a>
  <a href="/projects">projects</a>
  <a href="/quotes">quotes</a>
  <a href="/recepies">recepies</a>

  <button onclick={() => setLocale('en')}>en</button>

  <button onclick={toggleThemeMode}>
    {isDark ? '☀️' : '🌙'}
  </button>
</nav>

{@render children()}

<div style="display:none">
  {#each locales as locale (locale)}
    <a href={resolve(localizeHref(page.url.pathname, { locale }) as Path)}>{locale}</a>
  {/each}
</div>

<DemoLinks />
