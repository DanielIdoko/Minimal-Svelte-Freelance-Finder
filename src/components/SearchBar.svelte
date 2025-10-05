<script lang="ts">
	import { Search } from '@lucide/svelte';

	export let searchTerm: string;
	export let handleSearch: (query: string) => void;
</script>

<form
	class="sticky top-20 flex h-fit w-full items-center justify-center gap-3 bg-transparent p-2 transition duration-300 ease-in z-50"
>
	<input
		type="text"
		bind:value={searchTerm}
		placeholder="Search for jobs/gigs"
		class="w-full rounded-md border bg-white border-slate-700/15 p-2 text-xs text-slate-800 outline-0 md:w-96"
	/>
	<button
		class="flex cursor-pointer items-center justify-center gap-1 rounded-lg bg-blue-600 p-2 text-xs text-white shadow-xs hover:shadow-md"
		on:click={() => handleSearch(searchTerm)}
		>Search <Search size={12} /></button
	>
</form>
