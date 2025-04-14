<script lang="ts">
	import { page } from '$app/state';
	import { browser } from '$app/environment';
	import { fly } from 'svelte/transition';

	// State using runes
	let mobileMenuOpen = $state(false);

	// Reactive derived values
	const isPricingActive = $derived(page.url.pathname.startsWith('/pricing'));
	const isLocationsActive = $derived(page.url.pathname.startsWith('/locations'));
	const isHomeActive = $derived(page.url.pathname === '/');

	function toggleMobileMenu() {
		mobileMenuOpen = !mobileMenuOpen;
	}

	function closeMobileMenu() {
		mobileMenuOpen = false;
	}

	// Effect to handle body scroll lock when mobile menu is open
	$effect(() => {
		if (browser) {
			if (mobileMenuOpen) {
				document.body.style.overflow = 'hidden';
			} else {
				document.body.style.overflow = '';
			}
			// Cleanup function
			return () => {
				if (browser) document.body.style.overflow = '';
			};
		}
	});
</script>

<header class="sticky top-0 z-10 bg-white/80 backdrop-blur-md dark:bg-gray-900/80">
	<nav class="mx-auto flex h-12 items-center justify-between px-4 py-3 md:px-6">
		<a
			href="/"
			class="text-violet-300 md:text-2xl md:font-light dark:text-violet-500"
			onclick={closeMobileMenu}
		>
			stinapolerina
		</a>

		<div class="hidden items-center space-x-6 md:flex">
			<a
				href="/pricing"
				class="text-base font-medium transition-colors hover:text-violet-400 dark:hover:text-violet-400 {isPricingActive
					? 'text-violet-400 dark:text-violet-500'
					: 'text-gray-700 dark:text-gray-300'}"
			>
				Pricing
			</a>
			<a
				href="/locations"
				class="text-base font-medium transition-colors hover:text-violet-400 dark:hover:text-violet-400 {isLocationsActive
					? 'text-violet-400 dark:text-violet-500'
					: 'text-gray-700 dark:text-gray-300'}"
			>
				Locations
			</a>
			<!-- <div class="flex space-x-2">
				<Button
					variant="ghost"
					size="sm"
					onclick={() => changeLanguage('en')}
					addClass={currentLocale === 'en'
						? 'font-bold text-violet-400 dark:text-violet-500'
						: 'text-gray-500 dark:text-gray-400'}
					aria-label="Switch to English"
				>
					EN
				</Button>
				<Button
					variant="ghost"
					size="sm"
					onclick={() => changeLanguage('sv')}
					addClass={currentLocale === 'sv'
						? 'font-bold text-violet-400 dark:text-violet-500'
						: 'text-gray-500 dark:text-gray-400'}
					aria-label="Byt till Svenska"
				>
					SV
				</Button>
			</div> -->
		</div>

		<div class="flex items-center md:hidden">
			<button
				onclick={toggleMobileMenu}
				class="inline-flex cursor-pointer items-center justify-center rounded-md text-gray-700 hover:bg-gray-100 hover:text-violet-400 focus:ring-2 focus:ring-violet-500 focus:outline-none focus:ring-inset dark:text-gray-300 dark:hover:bg-gray-800 dark:hover:text-violet-400"
				aria-controls="mobile-menu"
				aria-expanded={mobileMenuOpen}
				aria-label="Open main menu"
			>
				<svg
					class="block h-6 w-6"
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M4 6h16M4 12h16m-7 6h7"
					/>
				</svg>
			</button>
		</div>
	</nav>

	{#if mobileMenuOpen}
		<div
			class="fixed inset-0 z-20 flex h-screen flex-col items-center justify-center bg-purple-50 md:hidden dark:bg-gray-900/95"
			id="mobile-menu"
			role="menu"
			in:fly={{ y: -200, duration: 300 }}
			out:fly={{ y: -200, duration: 300 }}
		>
			<button
				onclick={toggleMobileMenu}
				class="absolute top-4 right-4 inline-flex cursor-pointer items-center justify-center rounded-md p-2 text-gray-700 hover:bg-gray-100 hover:text-violet-400 focus:ring-2 focus:ring-violet-500 focus:outline-none focus:ring-inset dark:text-gray-300 dark:hover:bg-gray-800 dark:hover:text-violet-400"
				aria-controls="mobile-menu"
				aria-expanded={mobileMenuOpen}
				aria-label="Close main menu"
			>
				<svg
					class="block h-6 w-6"
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M6 18L18 6M6 6l12 12"
					/>
				</svg>
			</button>
			<div class="flex flex-col gap-y-8 p-4 text-center">
				<a
					href="/"
					class="block text-5xl font-semibold {isHomeActive
						? 'text-violet-400 dark:text-violet-500'
						: 'text-gray-800 dark:text-gray-200'} hover:text-violet-400 dark:hover:text-violet-400"
					onclick={closeMobileMenu}
				>
					stinapolerina
				</a>
				<a
					href="/pricing"
					class="block text-3xl font-semibold {isPricingActive
						? 'text-violet-400 dark:text-violet-500'
						: 'text-gray-800 dark:text-gray-200'} hover:text-violet-400 dark:hover:text-violet-400"
					onclick={closeMobileMenu}
				>
					Pricing
				</a>
				<a
					href="/locations"
					class="block text-3xl font-semibold {isLocationsActive
						? 'text-violet-400 dark:text-violet-500'
						: 'text-gray-800 dark:text-gray-200'} hover:text-violet-400 dark:hover:text-violet-400"
					onclick={closeMobileMenu}
				>
					Locations
				</a>
				<!-- <div class="flex justify-center space-x-4 pt-8">
					<Button
						variant="ghost"
						size="lg"
						onclick={() => changeLanguage('en')}
						addClass={currentLocale === 'en'
							? 'font-bold text-violet-400 dark:text-violet-500'
							: 'text-gray-500 dark:text-gray-400'}
						aria-label="Switch to English"
					>
						EN
					</Button>
					<Button
						variant="ghost"
						size="lg"
						onclick={() => changeLanguage('sv')}
						addClass={currentLocale === 'sv'
							? 'font-bold text-violet-400 dark:text-violet-500'
							: 'text-gray-500 dark:text-gray-400'}
						aria-label="Byt till Svenska"
					>
						SV
					</Button>
				</div> -->
			</div>
		</div>
	{/if}
</header>
