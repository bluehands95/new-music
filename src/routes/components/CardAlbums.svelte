<script>
	import Album from './Album.svelte';

	export let newAlbums;
</script>

<div class="md:col-span-2">
	{#each newAlbums as album}
		<Album {album} />
	{/each}
</div>
