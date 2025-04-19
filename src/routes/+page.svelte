<script lang="ts">
	import { page } from '$app/state';
	import ImageCarousel from '$lib/components/image-carousel.svelte';
	import Button from '$lib/components/button.svelte';
	import en from '$lib/i18n/en';
	import { fade } from 'svelte/transition';

	const imageModules = import.meta.glob('/static/images/*.{jpg,jpeg,png,webp,avif,gif}', {
		eager: true,
		query: 'url'
	});

	const carouselImages = Object.keys(imageModules)
		.map((path) => path.replace('/static', ''))
		.sort();
</script>

<svelte:head>
	<title>{en.seo.landing.title}</title>
	<meta name="description" content={en.seo.landing.description} />
	<meta property="og:title" content={en.seo.landing.title} />
	<meta property="og:description" content={en.seo.landing.description} />
	<meta property="og:url" content={page.url.href} />
	<meta property="og:type" content="website" />
</svelte:head>

<section
	class="relative flex min-h-[calc(100vh-var(--header-height,80px))] items-center justify-center overflow-hidden bg-gradient-to-br from-violet-200/80 via-white/80 to-purple-300/80 px-4 text-center dark:from-gray-800/90 dark:via-gray-900/90 dark:to-purple-900/30"
>
	{#if carouselImages.length > 0}
		<div class="absolute inset-0 h-full w-full">
			<ImageCarousel images={carouselImages} />
		</div>
	{/if}

	<div class="relative flex flex-col items-center" in:fade={{ duration: 500, delay: 200 }}>
		<h1
			class="mb-4 bg-gradient-to-r from-violet-400 to-purple-400 bg-clip-text text-6xl font-extrabold tracking-tighter text-transparent mix-blend-darken backdrop-filter sm:text-7xl md:text-8xl lg:text-9xl dark:from-violet-300 dark:via-purple-400 dark:to-violet-500 dark:mix-blend-soft-light"
		>
			{en.landing.title}
		</h1>
		<p class="mb-10 text-xl text-gray-500/80 drop-shadow md:text-2xl dark:text-gray-300">
			{en.landing.subtitle}
		</p>

		<div class="flex flex-col space-y-4 sm:flex-row sm:space-y-0 sm:space-x-6">
			<Button href="/pricing" variant="primary" size="lg">
				{en.landing.pricingLink}
			</Button>
			<Button href="/locations" variant="secondary" size="lg">
				{en.landing.locationsLink}
			</Button>
		</div>
	</div>
</section>

<style>
	/* Adjust header height variable if your header height changes */
	:root {
		--header-height: 48px; /* Example height, match your actual header */
	}
</style>
