<script>
	import { page } from '$app/stores';

	import '../app.css';
</script>

<header class="mb-20">
	<h1 class="text-3xl text-center font-bold text-slate-500 mb-10">
		Welcome to your Svelte/SvelteKit Playground.
	</h1>
	<h2 class="text-center">This is a place to practice and try everything Svelte related.</h2>
</header>

{#if $page.url.pathname !== '/'}
	<a class="underline" href="/">Go home</a>
{/if}

<div class="py-10 px-5 m-10 rounded">
	<slot />
</div>
