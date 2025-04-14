<script lang="ts">
	import { page } from '$app/state'; // Needed for Open Graph URL
	import en from '$lib/i18n/en';

	// Optional: Define card data structure if needed for loops later
	// type PricingOption = { title: string; price: string; description: string; }
</script>

<svelte:head>
	<title>{en.seo.pricing.title}</title>
	<meta name="description" content={en.seo.pricing.description} />
	<meta property="og:title" content={en.seo.pricing.title} />
	<meta property="og:description" content={en.seo.pricing.description} />
	<meta property="og:url" content={page.url.href} />
	<meta property="og:type" content="website" />
</svelte:head>

<div class="container mx-auto px-4 py-12 md:py-20">
	<div>
		<h1 class="text-center text-3xl font-bold tracking-tight sm:text-4xl md:text-5xl">
			{en.pricing.pageTitle}
		</h1>
		<h2
			class="tracking-loose mb-10 text-center text-xl text-violet-400 sm:text-xl md:mb-16 md:text-2xl"
		>
			{en.pricing.subtitle}
		</h2>
	</div>

	<div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-3">
		<div
			class="flex flex-col rounded-lg border border-transparent bg-white p-6 shadow-md transition-colors hover:border-violet-300 dark:bg-gray-800 dark:hover:border-pink-700"
		>
			<h2 class="mb-3 text-2xl font-semibold text-violet-400 dark:text-pink-400">
				{en.pricing.studio.title}
			</h2>
			<p class="mb-4 text-xl font-bold text-gray-800 dark:text-gray-200">
				{en.pricing.studio.price}
			</p>
			<p class="flex-grow text-gray-600 dark:text-gray-400">{en.pricing.studio.description}</p>
		</div>

		<div
			class="flex flex-col rounded-lg border border-transparent bg-white p-6 shadow-md transition-colors hover:border-violet-300 dark:bg-gray-800 dark:hover:border-pink-700"
		>
			<h2 class="mb-3 text-2xl font-semibold text-violet-400 dark:text-pink-400">
				{en.pricing.squad.title}
			</h2>
			<p class="mb-4 text-xl font-bold text-gray-800 dark:text-gray-200">
				{en.pricing.squad.price}
			</p>
			<p class="flex-grow text-gray-600 dark:text-gray-400">{en.pricing.squad.description}</p>
		</div>

		<div
			class="flex flex-col rounded-lg border border-transparent bg-white p-6 shadow-md transition-colors hover:border-violet-300 dark:bg-gray-800 dark:hover:border-pink-700"
		>
			<h2 class="mb-3 text-2xl font-semibold text-violet-400 dark:text-pink-400">
				{en.pricing.private.title}
			</h2>
			<p class="mb-4 text-xl font-bold text-gray-800 dark:text-gray-200">
				{en.pricing.private.price}
			</p>
			<p class="flex-grow text-gray-600 dark:text-gray-400">{en.pricing.private.description}</p>
		</div>
	</div>
</div>
