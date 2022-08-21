<script>
	import axios from 'axios';
	import { onMount } from 'svelte';
	import { Buffer } from 'buffer';

	const clientId = 'ef562f5531dd4903a011992105bbb2f6';
	const clientSecret = 'ff599668a9a14ea0a573001b7bd237b8';

	let token;
	let newAlbums;

	async function getNewReleases() {
		axios('https://api.spotify.com/v1/browse/new-releases', {
			method: 'GET',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json',
				Authorization: 'Bearer ' + token
			}
		})
			.then((res) => {
				newAlbums = res.data.albums.items;
				console.log(newAlbums);
			})
			.catch((err) => {
				console.log(err);
			});
	}

	$: if (token) {
		getNewReleases();
	}

	async function getAccessToken() {
		axios('https://accounts.spotify.com/api/token', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/x-www-form-urlencoded',
				Authorization: 'Basic ' + Buffer.from(clientId + ':' + clientSecret).toString('base64')
			},
			data: 'grant_type=client_credentials'
		})
			.then((res) => {
				token = res.data.access_token;
			})
			.catch((err) => {
				console.log(err);
			});
	}

	onMount(() => {
		getAccessToken();
	});
</script>

{#if newAlbums}
	<div class="bg-cultured">
		<div class="absolute text-black m-4">
			<i class="fa-solid fa-bars fa-bars-staggered w-10 fa-3x cursor-pointer" />
			<i class="fa-solid fa-xmark fa-3x cursor-pointer" />
		</div>
		{#each newAlbums as album}
			<div class="flex flex-wrap text-center text-black font-title uppercase">
				<div class="w-screen">
					{#each album.artists as artist}
						<h1 class="sm:text-6xl text-4xl">
							{artist.name}
						</h1>
					{/each}
				</div>
				<h1 class="sm:text-4xl text-2xl my-5 flex-auto">
					{album.name}
				</h1>
			</div>
		{/each}
	</div>
{/if}
