<script lang="ts">
	import { ArrowRightIcon } from '@lucide/svelte';
	export let job: Record<string, any>;
</script>

<div
	class="relative min-h-[165px] w-full cursor-pointer rounded-lg border border-slate-800/16 p-2 hover:shadow-sm"
>
	<div class="flex h-fit w-full items-center">
		<h3 class="text-md flex-1 font-semibold text-black">
			{job['project-title'].slice(0, 18) + '...'}
		</h3>
		<span class="rounded-full bg-black/5 px-2 py-1 text-xs text-slate-700">
			{job['project-tags']?.length > 30
				? job['project-tags'].slice(0, 14) + '...'
				: job['project-tags']}
		</span>
	</div>

	<p class="line-clamp-3 py-1 text-sm text-black/60">
		{job['project-description'].slice(0, 300)}
	</p>

	<div class="absolute bottom-3 text-xs w-full">
			<p class="py-1 text-black/40">{job['ends in']}</p>
		
		<div class="w-full h-fit flex items-center ">
			<p class="text-black flex-1">{job['project-price']}</p>
			<a
				href={job['project-link'] || '#'}
				target="_blank"
				class="flex items-center gap-1 text-xs text-blue-600 hover:underline absolute right-6"
			>
				View <ArrowRightIcon size={13} />
			</a>
		</div>
	</div>
</div>
