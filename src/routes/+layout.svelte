<script lang="ts">
	import Header from '$lib/components/header.svelte';
	import '../app.css';

	let { children } = $props();
</script>

<div class="flex min-h-screen flex-col">
	<Header />

	<main class="flex-grow">
		{@render children()}
	</main>
</div>
