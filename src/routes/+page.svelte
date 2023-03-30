<script lang="ts">
	async function getRandomPokimon() : {
		url: string,
		name: string
	}{
		let randomId = Math.floor(Math.random() * 898) + 1;
		let result = await fetch("https://pokeapi.co/api/v2/pokemon/" + randomId);
		const json = await result.json()
		if(result.ok) {
			return {
				url: json.sprites.front_default,
				name: json.name
			}
		}
		return getRandomPokimon()
	}

	let randomNumberPromise = getRandomPokimon()

	function updateRandomNumber() {
		randomNumberPromise = getRandomPokimon()
	}
</script>

<div class="flex flex-col text-white container justify-center min-h-screen items-center mx-auto">
	<p>	Here's a random Pokemon:
	Poki!
</p>
{#await randomNumberPromise}
	<p>Getting a random poki...</p>
{:then poki}
		<img src={poki.url} alt="poki" class="pixelated">
	<p>{poki.name}</p>
{:catch error}
	<p>{error.message}</p>
{/await}
		<button on:click={updateRandomNumber} class="my-10 bg-rose-700 rounded-md hover:opacity-80 px-3 py-1">Reroll</button>
</div>

<style lang="postcss">
  :global(html) {
    background-color: theme(colors.gray.800);
  }
</style>
