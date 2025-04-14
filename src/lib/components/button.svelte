<script lang="ts">
	import type { Snippet } from 'svelte';
	import type { HTMLAttributes } from 'svelte/elements';

	interface ButtonProps extends HTMLAttributes<HTMLButtonElement | HTMLAnchorElement> {
		href?: string;
		variant?: 'primary' | 'secondary' | 'ghost';
		size?: 'sm' | 'md' | 'lg';
		addClass?: string;
		children: Snippet<[]>;
	}

	// Using runes
	let {
		href = undefined as string | undefined,
		variant = 'primary' as 'primary' | 'secondary' | 'ghost',
		size = 'md' as 'sm' | 'md' | 'lg',
		addClass = '' as string, // For additional Tailwind classes
		children,
		...restProps // Pass other props like type="submit" etc.
	}: ButtonProps = $props();

	// Base styles
	const base =
		'inline-flex shadow items-center justify-center rounded-md font-semibold transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none ring-offset-background';

	// Variant styles
	const variants = {
		primary: 'bg-violet-300 border border-violet-400 text-white hover:bg-violet-400/90',
		secondary:
			'bg-gray-400/80 border border-gray-500 text-white hover:bg-gray-400 dark:bg-gray-300 dark:text-gray-900 dark:hover:bg-gray-400/90',
		ghost: 'hover:bg-gray-200/50 dark:hover:bg-gray-700/50'
	};

	// Size styles
	const sizes = {
		sm: 'h-9 px-3 text-sm',
		md: 'h-10 px-4 py-2 text-base',
		lg: 'h-11 px-8 text-lg'
	};

	let computedClass = $derived(`${base} ${variants[variant]} ${sizes[size]} ${addClass}`);
</script>

{#if href}
	<a {href} class={computedClass} {...restProps}>
		{@render children()}
	</a>
{:else}
	<button class={computedClass} {...restProps}>
		{@render children()}
	</button>
{/if}
