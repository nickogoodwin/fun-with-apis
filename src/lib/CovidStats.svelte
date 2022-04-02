<script>
	import axios from 'axios';
	import { onMount } from 'svelte';

	let selectedCountry;
	let data;
	let globalData;
	let countriesData;
	let currentData;

	let getCovidData = async () => {
		let url = `https://api.covid19api.com/summary`;
		await axios.get(url).then((response) => {
			data = response.data;
		});
		globalData = data.Global;
		countriesData = data.Countries;
		currentData = globalData;
	};

	let updateStats = (selectedCountry) => {
		//if 'global' is selected, show global stats
		if (selectedCountry == 'global') {
			currentData = globalData;
			return;
		}

		//otherwise, show stats for selected country
		let selectedCountryData;
		for (let i = 0; i < countriesData.length; i++) {
			if (countriesData[i].Country == selectedCountry) {
				selectedCountryData = countriesData[i];
			}
		}
		currentData = selectedCountryData;
	};

	onMount(() => {
		getCovidData();
	});
</script>

<div
	class="flex flex-col rounded-2xl p-4 backdrop-blur-md bg-white bg-opacity-50 shadow-lg shadow-neutral-900"
>
	<h1 class="text-center text-2xl m-4">Covid-19 Stats</h1>

	<div class="flex-auto">
		<p>
			Get current Covid-19 information for a given country (default: Global
			data)
		</p>
	</div>

	<div class="flex-auto">
		{#if countriesData}
			<label class="block" for="country">Select Country</label>
			<select
				class="w-full p-2 rounded-md border border-black"
				name="country"
				id="country"
				bind:value={selectedCountry}
				on:change={() => updateStats(selectedCountry)}
			>
				<option value="global">--GLOBAL--</option>
				{#each countriesData as country}
					<option value={country.Country}>{country.Country}</option>
				{/each}
			</select>
		{/if}
	</div>

	<div class="flex-auto">
		{#if currentData}
			<div class="global-data">
				<h3>
					Total Cases: <span class="font-bold"
						>{currentData.TotalConfirmed.toLocaleString('en-us')}</span
					>
				</h3>
				<p>
					New Confirmed Cases Today: <span class="font-bold"
						>{currentData.NewConfirmed.toLocaleString('en-us')}</span
					>
				</p>
				<p>
					Total Deaths: <span class="font-bold"
						>{currentData.TotalDeaths.toLocaleString('en-us')}</span
					>
				</p>
				<p>
					New Deaths: <span class="font-bold"
						>{currentData.NewDeaths.toLocaleString('en-us')}</span
					>
				</p>
			</div>
		{/if}
	</div>

	<div class="mb-0 mt-2 text-center w-full">
		<p class="text-sm text-gray-800">
			API provided by <a class="hover:underline" href="https://covid19api.com/"
				>covid19api.com</a
			>
		</p>
	</div>
</div>
