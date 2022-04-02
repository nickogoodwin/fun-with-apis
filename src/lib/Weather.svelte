<script>
	import axios from 'axios';

	let states = [
		'AL',
		'AK',
		'AS',
		'AZ',
		'AR',
		'CA',
		'CO',
		'CT',
		'DE',
		'DC',
		'FM',
		'FL',
		'GA',
		'GU',
		'HI',
		'ID',
		'IL',
		'IN',
		'IA',
		'KS',
		'KY',
		'LA',
		'ME',
		'MH',
		'MD',
		'MA',
		'MI',
		'MN',
		'MS',
		'MO',
		'MT',
		'NE',
		'NV',
		'NH',
		'NJ',
		'NM',
		'NY',
		'NC',
		'ND',
		'MP',
		'OH',
		'OK',
		'OR',
		'PW',
		'PA',
		'PR',
		'RI',
		'SC',
		'SD',
		'TN',
		'TX',
		'UT',
		'VT',
		'VI',
		'VA',
		'WA',
		'WV',
		'WI',
		'WY',
	];
	let city, state;
	let weatherData;
	let weatherIcon;

	const getWeatherInfo = async () => {
		let api_key = `36d1e439a926317fb3007117895259ac`;

		// get latitude and longitude of entered location
		const geocodeURL = `http://api.openweathermap.org/geo/1.0/direct`;
		const weatherInfoURL = `https://api.openweathermap.org/data/2.5/weather`;

		await axios
			.get(geocodeURL, {
				params: {
					q: `${city},${state},us`,
					appid: api_key,
					limit: 1,
				},
			})
			.then((response) => {
				return axios.get(weatherInfoURL, {
					params: {
						lat: response.data[0].lat,
						lon: response.data[0].lon,
						appid: api_key,
						units: 'imperial',
					},
				});
			})
			.then((response) => {
				weatherData = response.data;
			});

		//you left off here!!!!!
		console.log(weatherData);
		weatherIcon = `https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`;
	};
</script>

<div
	class="flex flex-col rounded-2xl p-4 backdrop-blur-md bg-white bg-opacity-50 shadow-lg shadow-neutral-900"
>
	<h1 class="text-center text-2xl m-4">Weather</h1>

	<div class="flex-auto">
		<p>Get current weather information for a city in the United States.</p>
	</div>

	<div class="flex-auto">
		<label class="block" for="city-input">City</label>
		<div class="mt-1 relative rounded-md shadow-sm">
			<input
				class="w-full p-2 rounded-md border border-black"
				type="text"
				name="city-input"
				id="city-input"
				bind:value={city}
			/>
		</div>
	</div>

	<div class="flex-auto">
		<label class="block" for="state-selector">State: </label>
		<div class="mt-1 relative rounded-md shadow-sm">
			<select
				class="w-full py-2 rounded-md border border-black"
				name="state-selector"
				id="state-selector"
				bind:value={state}
			>
				{#each states as st}
					<option value={st}>{st}</option>
				{/each}
			</select>
		</div>
	</div>

	<div class="flex-auto text-center">
		<input
			class="px-4 py-2 my-2 text-white bg-gray-900 rounded-md hover:bg-gray-700 hover:cursor-pointer"
			type="button"
			value="Get Weather Info"
			on:click={getWeatherInfo}
		/>
	</div>

	<div class="flex-auto grid grid-cols-2 my-4">
		{#if weatherData}
			<div class="flex align-end">
				<img src={weatherIcon} alt="" />
			</div>

			<div>
				<h1 class="text-xl font-bold">{weatherData.main.temp} &deg; F</h1>
				<p>"{weatherData.weather[0].description}"</p>
				<p>Humidity: {weatherData.main.humidity}%</p>
				<p>Wind Speed: {weatherData.wind.speed} mph</p>
			</div>
		{/if}
	</div>

	<div class="mb-0 mt-2 text-center w-full">
		<p class="text-sm text-gray-800">
			API provided by <a
				class="hover:underline"
				href="https://openweathermap.org/">openweathermap.org</a
			>
		</p>
	</div>
</div>
