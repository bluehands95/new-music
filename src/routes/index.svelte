<script>
	import axios from 'axios';
	import { onMount } from 'svelte';
	import { Buffer } from 'buffer';
	import { currentCountry } from './stores';

	import Menu from './components/Menu.svelte';
	import CardAlbums from './components/CardAlbums.svelte';

	const clientId = 'ef562f5531dd4903a011992105bbb2f6';
	const clientSecret = 'ff599668a9a14ea0a573001b7bd237b8';

	let token;
	let newAlbums;

	$: url = `https://api.spotify.com/v1/browse/new-releases?country=${$currentCountry}`;

	$: if (token) {
		setInterval(() => {
			getNewReleases($currentCountry);
		}, 300);
	}

	$: if (currentCountry) {
		setInterval(() => {
			getNewReleases($currentCountry);
		}, 300);
	}

	async function getNewReleases() {
		axios(url, {
			method: 'GET',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json',
				Authorization: 'Bearer ' + token
			}
		})
			.then((res) => {
				newAlbums = res.data.albums.items;
			})
			.catch((err) => {
				console.log(err);
			});
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
	<div class="grid md:grid-cols-3 gap-3 cool">
		<Menu {currentCountry} {token} />
		<CardAlbums {newAlbums} />
	</div>
{/if}

<style>
	.cool {
		background-color: #f9564f;
		background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='120' height='120' viewBox='0 0 120 120'%3E%3Cpolygon fill='%23000' fill-opacity='.1' points='120 0 120 60 90 30 60 0 0 0 0 0 60 60 0 120 60 120 90 90 120 60 120 0'/%3E%3C/svg%3E");
	}
</style>
