<script lang="ts">
	import { onMount } from 'svelte';
	import Header from '../components/Header.svelte';
	import Job from '../components/Job.svelte';
	import { BASEURL, options } from '../utils/api';
	import { ArrowRightIcon, Loader } from '@lucide/svelte';

	let jobs: any[] = [];
	let allJobs: any[] = []; // keep an unfiltered copy
	let errorMessage = '';
	let isLoading = false;
	let category: string = '';

	// Fetch jobs from API
	async function fetchJobs() {
		isLoading = true;
		try {
			const response = await fetch(`${BASEURL}`, options);
			const result = await response.json();

			// Append jobs to allJobs
			jobs = result.posts || [];
			allJobs = [...jobs];
			errorMessage = '';
		} catch (error: any) {
			errorMessage = error.message || 'Failed to load jobs';
			console.error(error);
		} finally {
			isLoading = false;
		}
	}

	// Filter by category
	function filterByCategory(query: string) {
		if (query.length < 0) {
			allJobs = [...jobs];
			errorMessage = '';
			return;
		}
		// console.log(allJobs);
		// Search for jobs
		const searchResults = allJobs.filter((job) => {
			const tags = job['project-tags']?.toLowerCase() || '';
			return tags.includes(query.toLowerCase());
		});

		console.log(searchResults);
		allJobs = [...searchResults];

		if (searchResults.length === 0) {
			errorMessage = `No jobs found for "${query}"`;
			jobs = [];
		} else {
			errorMessage = '';
			jobs = searchResults;
		}
	}

	// Fetch on mount
	onMount(fetchJobs);

	// Reactively filter when user types
	$: if (category !== '') {
		filterByCategory(category);
	}
</script>

<main class="h-full w-full px-2 py-10 font-normal">
	<Header />

	<h2 class="mt-8 p-1 px-2 text-[22px] text-black">Find Freelance Jobs</h2>

	<!-- Search Bar -->
	<div
		class="sticky top-20 z-30 flex h-fit w-full flex-col items-center justify-center gap-3 rounded-lg border border-slate-800/15 bg-white p-2 shadow-md md:h-12 md:flex-row"
	>
		<div class="flex h-fit w-full items-center gap-2 md:mt-0">
			<p class="text-sm text-black/50 md:ml-2">Category:</p>
			<input
				type="text"
				placeholder="UI/UX, Product Management..."
				class="w-full rounded-md border border-slate-100 p-1 text-sm focus:outline-slate-100 md:ml-3"
				bind:value={category}
			/>
		</div>
	</div>

	<!-- Loading/Error States -->
	{#if isLoading}
		<div class="flex h-full w-full items-center justify-center p-1">
			<p class="mt-4 flex items-center justify-center gap-2 text-black/60">
				<Loader size={20} /> Loading
			</p>
		</div>
	{:else if errorMessage}
		<p class="mt-4 text-center text-red-500">{errorMessage}</p>
	{:else if jobs.length === 0}
		<p class="mt-4 text-center text-2xl text-black/70">Sorry, no jobs were found</p>
	{/if}

	<!-- Job List -->
	<div class="mt-10 grid h-full w-full grid-cols-1 gap-5 md:grid-cols-2 lg:grid-cols-3">
		{#each jobs as job (job.id || Math.random())}
			<Job {job} />
		{/each}
	</div>
</main>
