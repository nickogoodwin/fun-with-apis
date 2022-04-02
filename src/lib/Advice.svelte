<script>
	import axios from 'axios';

	let advice;
	let getAdvice = async () => {
		let adviceURL = `https://api.adviceslip.com/advice`;
		await axios
			.get(adviceURL)
			.then((response) => response.data)
			.then((data) => {
				advice = data.slip.advice;
			});
	};
</script>

<div
	class="flex flex-col rounded-2xl p-4 backdrop-blur-md bg-white bg-opacity-50 shadow-lg shadow-neutral-900"
>
	<div class="flex-auto">
		<h1 class="text-center text-2xl m-4">Unsolicited Advice</h1>
	</div>

	<div class="flex-auto text-center">
		{#if !advice}
			<p>You didn't ask for it, but here it is.</p>
		{:else}
			<p>{advice}</p>
		{/if}
	</div>

	<div class="text-center">
		<input
			class="px-4 py-2 my-2 text-white bg-gray-900 rounded-md hover:bg-gray-700 hover:cursor-pointer"
			type="button"
			value="Get Advice"
			on:click={getAdvice}
		/>
	</div>

	<div class="mb-0 mt-2 text-center w-full">
		<p class="text-sm text-gray-800">
			API provided by <a
				class="hover:underline"
				href="https://api.adviceslip.com/">api.adviceslip.com</a
			>
		</p>
	</div>
</div>
