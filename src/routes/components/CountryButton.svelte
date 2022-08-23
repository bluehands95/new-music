<script>
	export let currentCountry;
	export let country;
	const changeCountry = (country) => {
		currentCountry.set(country);
		document.body.scrollIntoView();
	};
</script>

<li
	class="bg-cultured m-2 p-2 border-2 text-2xl cursor-pointer rounded-lg uppercase"
	class:selected={$currentCountry === country}
	on:click={() => changeCountry(country)}
>
	{country}
</li>

<style>
	.selected {
		color: #f9564f;
		background-color: #0c0a3e;
		border: 2px solid #0c0a3e;
		border-radius: 0 0 15px 0;
	}
</style>
