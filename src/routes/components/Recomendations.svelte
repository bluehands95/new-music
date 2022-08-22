<script>
	import axios from 'axios';
	export let token;

	let seed_artists = '';
	let seed_tracks = '';
	let seed_genres = '';

	let recomendations;

	$: url = `https://api.spotify.com/v1/recommendations?limit=5&market=ES&seed_artists=${seed_artists}&seed_genres=${seed_genres}&seed_tracks=${seed_tracks}`;

	async function getRecomendations() {
		axios(url, {
			method: 'GET',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json',
				Authorization: 'Bearer ' + token
			}
		})
			.then((res) => {
				recomendations = res.tracks;
			})
			.catch((err) => {
				console.log(err);
			});
	}
</script>

<div class="flex justify-center flex-wrap font-title text-black w-full mt-10 mb-10">
	<input
		bind:value={seed_artists}
		class="w-3/4 p-3 sq-button text-lg uppercase my-2"
		type="text"
		placeholder="Seed artists"
	/>
	<input
		bind:value={seed_tracks}
		class="w-3/4 p-3 sq-button text-lg uppercase my-2"
		type="text"
		placeholder="Seed Tracks"
	/>
	<input
		bind:value={seed_genres}
		class="w-3/4 p-3 sq-button text-lg uppercase my-2"
		type="text"
		placeholder="Seed Genres"
	/>
	<button
		class="uppercase sq-button text-lg mt-2 bg-black text-cultured p-2"
		on:click={getRecomendations}>Get Recomendations</button
	>

	{#if recomendations}
		{#each recomendations as reco}
			<h1>{reco.album.artists[0].name}</h1>
		{/each}
	{/if}
</div>

<style>
	.sq-button {
		border: 2px solid #0c0a3e;
		border-radius: 0 0 15px 0;
	}
</style>
