<script lang="ts">
	import { page } from '$app/state';
	import en from '$lib/i18n/en';
</script>

<svelte:head>
	<title>{en.seo.locations.title}</title>
	<meta name="description" content={en.seo.locations.description} />
	<meta property="og:title" content={en.seo.locations.title} />
	<meta property="og:description" content={en.seo.locations.description} />
	<meta property="og:url" content={page.url.href} />
	<meta property="og:type" content="website" />
</svelte:head>

<div class="container mx-auto px-4 py-12 text-center md:py-20">
	<h1 class="mb-8 text-3xl font-bold tracking-tight sm:text-4xl md:text-5xl">
		{en.locations.pageTitle}
	</h1>
	<p class="text-lg text-gray-600 dark:text-gray-400">
		{en.locations.placeholder}
	</p>
</div>
