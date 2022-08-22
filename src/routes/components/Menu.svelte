<script>
	import CountryButton from './CountryButton.svelte';
	import Recomendations from './Recomendations.svelte';

	export let currentCountry;
	export let token;

	let countries = ['NZ', 'MX', 'US', 'AR', 'DE', 'CA', 'NL', 'FR'];
</script>

<div
	class="md:col-span-1 md:sticky md:top-0 md:h-32 text-center text-6xl text-black font-title uppercase"
>
	<h1>Menu</h1>
	<h2 class="text-4xl mt-5">Country</h2>
	<ul class="flex justify-center flex-wrap textl-3xl m-1">
		{#each countries as country}
			<CountryButton {country} {currentCountry} />
		{/each}
	</ul>
	<Recomendations {token} />
</div>
