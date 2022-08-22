<script>
	import { fly, fade } from 'svelte/transition';

	export let album;
</script>

<div
	class="text-center text-black font-title uppercase"
	in:fly={{ y: -100, duration: 900 }}
	out:fade
>
	<div class="">
		{#each album.artists as artist}
			<h1 class="sm:text-6xl text-4xl">
				{artist.name}
			</h1>
		{/each}
	</div>
	<h1 class="sm:text-4xl text-2xl mb-5">
		{album.name}
	</h1>
</div>
