<script>
	import axios from 'axios';

	//populate years dropdown
	let present = new Date().getFullYear();
	let past = present - 100;

	let years = [];
	for (let i = present; i >= past; i--) {
		years.push(i);
	}

	let selectedYear;
	let amount;
	let inflationData;

	//calculate
	let calculate = async () => {
		let url = `https://www.statbureau.org/calculate-inflation-price-json`;
		await axios
			.get(url, {
				params: {
					country: `united-states`,
					start: `${selectedYear}/1/1`,
					end: `${present}/1/1`,
					amount: `${amount}`,
				},
			})
			.then((response) => (inflationData = response.data))
			.catch((err) => {
				console.log(err);
			});
	};
</script>

<div
	class="flex flex-col rounded-2xl p-4 backdrop-blur-md bg-white bg-opacity-50 shadow-lg shadow-neutral-900"
>
	<h1 class="text-center text-2xl m-4">Inflation Calculator</h1>

	<div class="flex-auto">
		<p>
			Calculate how much a given dollar ($) amount from a given year would be
			worth today.
		</p>
	</div>

	<div class="flex-auto">
		<label for="amount-input" class="block">Amount:</label>
		<div class="mt-1 relative rounded-md shadow-sm">
			<div
				class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
			>
				<span class="text-gray-500 sm:text-sm">$</span>
			</div>
			<input
				class="w-full pl-8 py-2 rounded-md border border-black"
				type="text"
				name="amount-input"
				id="amount-input"
				placeholder="0.00"
				bind:value={amount}
			/>
		</div>
	</div>

	<div class="flex-auto">
		<label for="year" class="block">From the Year:</label>
		<select
			class="w-full p-2 rounded-md border border-black"
			name="year-select"
			id="year-select"
			bind:value={selectedYear}
		>
			{#each years as year}
				<option value={year}>{year}</option>
			{/each}
		</select>
	</div>

	<div class="flex-auto text-center">
		<input
			class="px-4 py-2 my-2 text-white bg-gray-900 rounded-md hover:bg-gray-700 hover:cursor-pointer"
			on:click={calculate}
			type="button"
			value="Calculate for Inflation"
		/>
	</div>

	<div class="flex-auto text-center">
		{#if inflationData}
			<p>Would be worth:</p>

			<p><span class="text-green-900 font-bold">{inflationData}</span> today</p>
		{/if}
	</div>

	<div class="mb-0 text-center w-full">
		<p class="text-sm text-gray-800">
			API provided by <a
				class="hover:underline"
				href="https://www.statbureau.org">www.statbureau.org</a
			>
		</p>
	</div>
</div>
