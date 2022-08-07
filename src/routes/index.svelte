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

<div class="w-full flex p-4 my-4 justify-center align-middle text-black">
	<p class="text-4xl font-title">New Releases</p>
</div>
{#if newAlbums}
	<div class="flex flex-wrap mx-4 justify-center my-4">
		{#each newAlbums as album}
			<div class="grid text-black m-2 p-3">
				<div class="flex-auto">
					<img src={album.images[1].url} alt="" width="210px" />
				</div>
				<div class="flex-auto my-3">
					<div class="flex">
						<h1 class="bold text-lg truncate my-1 w-32">{album.name}</h1>
					</div>
					<div class="flex flex-col">
						{#each album.artists as artist}
							<h1 class="text-zinc-800 text-base">{artist.name}</h1>
						{/each}
					</div>
				</div>
			</div>
		{/each}
	</div>
{/if}
