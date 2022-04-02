<script>
	import './app.css';
	import InflationCalculator from './lib/InflationCalculator.svelte';
	import Weather from './lib/Weather.svelte';
	import CovidStats from './lib/CovidStats.svelte';
	import GuessingGame from './lib/GuessingGame.svelte';
	import Advice from './lib/Advice.svelte';
</script>

<main class="flex min-h-screen flex-col bg-gray-200 p-4">
	<div class="m-4 text-3xl">
		<h1 class="text-white">Fun with APIs</h1>
	</div>
	<div class="flex flex-grow content-center items-center">
		<div
			class="grid gap-4 sm:grid-cols-1 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5"
		>
			<InflationCalculator />
			<Weather />
			<CovidStats />
			<GuessingGame />
			<Advice />
		</div>
	</div>
	<div class="text-center my-4">
		<p class="text-white">Created by Nicko Goodwin</p>
	</div>
</main>

<style>
	:root {
		font-family: Ubuntu, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
			Oxygen, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
	}

	:global(body) {
		margin: 0;
	}

	main {
		background-image: url('./static/forest.jpg');
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
		background-attachment: fixed;
	}
</style>
