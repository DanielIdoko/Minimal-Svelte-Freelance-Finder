<script lang="ts">
	import { Search } from '@lucide/svelte';
	import { getContext } from 'svelte';
	import type { Writable } from 'svelte/store';

	let searchBarVisible: Writable<boolean> = getContext('search-bar-visible');
</script>

<header
	class="sticky top-0 z-40 flex h-16 w-full items-center border border-t-0 border-r-0 border-l-0 border-b-slate-800/15 bg-white p-1 px-3"
>
	<a href="/" class="flex-1 text-xl font-bold text-blue-500">Elevare</a>
	<nav class="h-fit w-fit p-2">
		<button
			class="cursor-pointer rounded-lg border-none border-slate-800 p-1 text-black transition duration-300 hover:bg-black/5"
			on:click={() => searchBarVisible.update((visible) => !visible)}
		>
			<Search size="20" />
		</button>
	</nav>
</header>
