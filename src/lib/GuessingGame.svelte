<script>
	import axios from 'axios';
	import { onMount } from 'svelte';

	let src = '../src/assets/suits.png';
	let playing = false,
		guessing = false,
		correctGuess = false;
	let score = 0,
		remainingCards = 0;
	let deck, card, suit;

	onMount(async () => {
		//get a new deck of cards
		let newDeckURL = `https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1`;
		deck = await axios
			.get(newDeckURL)
			.then((response) => response.data)
			.then((data) => {
				return data;
			})
			.catch((err) => {
				console.error('Could not retrieve a Deck of cards', err);
			});
	});

	let startGame = () => {
		playing = true;
	};

	let makeGuess = async () => {
		guessing = true;

		// start the game by drawing a card
		let drawCardURL = `https://deckofcardsapi.com/api/deck/${deck.deck_id}/draw/?count=1`;
		card = await axios
			.get(drawCardURL)
			.then((response) => response.data)
			.then((data) => {
				return data;
			})
			.catch((err) => {
				console.error('Could not draw a card', err);
			});

		remainingCards = card.remaining;

		if (remainingCards > 0) {
			src = card.cards[0].image;
			if (suit === card.cards[0].suit) {
				correctGuess = true;
				score++;
			} else {
				correctGuess = false;
			}
		} else {
			playing = false;
		}
	};
</script>

<div
	class="flex flex-col rounded-2xl p-4 backdrop-blur-md bg-white bg-opacity-50 shadow-lg shadow-neutral-900"
>
	<div class="m-2 text-xl">
		<h1 class="text-center text-2xl m-4">Guessing Game</h1>
	</div>

	<div class="flex-auto">
		<img class="mx-auto" {src} alt="card" />
	</div>

	<div class="flex-auto text-center">
		{#if playing}
			<h2 class="text-xl">
				Score: <span class="font-bold">{score}</span>
			</h2>
			<h2 class="text-xl">
				Cards Remaining: <span class="font-bold">{remainingCards}</span>
			</h2>
		{/if}
	</div>

	<div class="flex-auto my-2">
		{#if !playing}
			<div class="text-center">
				<input
					class="px-4 py-2 my-2 text-white bg-gray-900 rounded-md hover:bg-gray-700 hover:cursor-pointer"
					type="button"
					value="Play"
					on:click={startGame}
				/>
			</div>
		{:else}
			<div>
				<h1>Guess the suit of the next card that will be drawn</h1>
				<div class="flex flex-col">
					<label>
						<input type="radio" bind:group={suit} name="suits" value="SPADES" />
						♠️ Spades
					</label>
					<label>
						<input type="radio" bind:group={suit} name="suits" value="HEARTS" />
						♥️ Hearts
					</label>
					<label>
						<input
							type="radio"
							bind:group={suit}
							name="suits"
							value="DIAMONDS"
						/>
						♦️ Diamonds
					</label>
					<label>
						<input type="radio" bind:group={suit} name="suits" value="CLUBS" />
						♣️ Clubs
					</label>

					<div class="text-center">
						{#if guessing && correctGuess}
							<h2 class="text-green-600">Correct!</h2>
						{:else if guessing && !correctGuess}
							<h2 class="text-red-600">Incorrect :(</h2>
						{/if}
					</div>
					<input
						class="px-4 py-2 my-2 text-white bg-gray-900 rounded-md hover:bg-gray-700 hover:cursor-pointer"
						type="button"
						value="Guess"
						on:click={makeGuess}
					/>
				</div>
			</div>
		{/if}
	</div>

	<div class="mb-0 mt-2 text-center w-full">
		<p class="text-sm text-gray-800">
			API provided by <a
				class="hover:underline"
				href="https://deckofcardsapi.com/">deckofcardsapi.com</a
			>
		</p>
	</div>
</div>

<style>
</style>
