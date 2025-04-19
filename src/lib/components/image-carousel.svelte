<script lang="ts">
	import { onMount, onDestroy } from 'svelte';

	interface Props {
		images?: string[];
		duration?: number;
		autoplay?: boolean;
	}

	let { images = [], duration = 5000, autoplay = true }: Props = $props();

	let currentIndex = $state(0);
	let timer: ReturnType<typeof setTimeout>;
	let isPaused = $state(false);
	let progressValue = $state(0);
	let progressInterval: ReturnType<typeof setInterval>;

	// Create a new array with the length of images
	const indices = $derived(Array.from({ length: images.length }, (_, i) => i));

	function nextSlide() {
		currentIndex = (currentIndex + 1) % images.length;
		resetProgress();
	}

	function prevSlide() {
		currentIndex = (currentIndex - 1 + images.length) % images.length;
		resetProgress();
	}

	function goToSlide(index: number) {
		currentIndex = index;
		resetProgress();
	}

	function startTimer() {
		clearTimeout(timer);
		if (autoplay && !isPaused && images.length > 1) {
			timer = setTimeout(() => {
				nextSlide();
			}, duration);
		}
	}

	function resetProgress() {
		progressValue = 0;
		clearInterval(progressInterval);
		startProgress();
	}

	function startProgress() {
		if (autoplay && !isPaused && images.length > 1) {
			const step = 10; // Update every 10ms
			const increment = (step / duration) * 100;

			progressInterval = setInterval(() => {
				progressValue = Math.min(progressValue + increment, 100);

				if (progressValue >= 100) {
					clearInterval(progressInterval);
					// Explicitly move to the next slide when progress completes
					nextSlide();
				}
			}, step);
		}
	}

	function handleMouseEnter() {
		isPaused = true;
		clearTimeout(timer);
		clearInterval(progressInterval);
	}

	function handleMouseLeave() {
		isPaused = false;
		startTimer();
		startProgress();
	}

	onMount(() => {
		if (images.length > 0) {
			startTimer();
			startProgress();
		}
	});

	onDestroy(() => {
		clearTimeout(timer);
		clearInterval(progressInterval);
	});
</script>

<div class="relative h-full w-full overflow-hidden">
	{#each images as image, i (i)}
		<div
			class="absolute inset-0 h-full w-full transition-opacity duration-1000 ease-in-out"
			style="opacity: {currentIndex === i ? 1 : 0};"
		>
			<img src={image} alt="Carousel image {i + 1}" class="h-full w-full object-cover opacity-30" />
		</div>
	{/each}

	<!-- Navigation dots -->
	<div
		class="absolute bottom-6 left-1/2 flex -translate-x-1/2 space-x-2"
		onmouseenter={handleMouseEnter}
		onmouseleave={handleMouseLeave}
		role="region"
		aria-label="Image carousel navigation"
	>
		{#each indices as i (i)}
			<button
				aria-label="Go to slide {i + 1}"
				class="group flex h-8 cursor-pointer items-center"
				onclick={() => goToSlide(i)}
			>
				<div
					class="h-[3px] w-2 overflow-hidden rounded-full bg-white/60 transition-all group-hover:bg-white/80 {currentIndex ===
					i
						? 'w-12 bg-white'
						: ''}"
				>
					{#if currentIndex === i}
						<div class="h-full bg-white transition-all" style="width: {progressValue}%;"></div>
					{/if}
				</div>
			</button>
		{/each}
	</div>

	<!-- Previous button -->
	<button
		class="absolute top-1/2 left-4 -translate-y-1/2 cursor-pointer rounded-full bg-black/30 p-2 text-white opacity-70 transition-opacity hover:opacity-100"
		onclick={prevSlide}
		aria-label="Previous slide"
		onmouseenter={handleMouseEnter}
		onmouseleave={handleMouseLeave}
	>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			fill="none"
			viewBox="0 0 24 24"
			stroke-width="1.5"
			stroke="currentColor"
			class="h-6 w-6"
		>
			<path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5L8.25 12l7.5-7.5" />
		</svg>
	</button>

	<!-- Next button -->
	<button
		class="absolute top-1/2 right-4 -translate-y-1/2 cursor-pointer rounded-full bg-black/30 p-2 text-white opacity-70 transition-opacity hover:opacity-100"
		onclick={nextSlide}
		aria-label="Next slide"
		onmouseenter={handleMouseEnter}
		onmouseleave={handleMouseLeave}
	>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			fill="none"
			viewBox="0 0 24 24"
			stroke-width="1.5"
			stroke="currentColor"
			class="h-6 w-6"
		>
			<path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
		</svg>
	</button>
</div>
