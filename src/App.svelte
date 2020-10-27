<script>
	import {fade} from 'svelte/transition';
	import {scale} from 'svelte/transition';
	
	console.log(localStorage.cards);
	if (localStorage.cards == undefined || localStorage.cards == null) {
		localStorage.cards = JSON.stringify([]);
	}
	var cards = JSON.parse(localStorage.cards);
	
	if (localStorage.dark_mode == undefined || localStorage.dark_mode == null) {
		localStorage.dark_mode = JSON.stringify(false);
	}
	var dark_mode = JSON.parse(localStorage.dark_mode);
	
	if (dark_mode == true) {
		window.document.body.classList.toggle('dark-mode');
	}
	
	function add_card() {
		cards.push("");
		cards[0] = cards[0];
	}
	
	// called after editing text and clicking off the textarea
	function update(index) {
		const b = document.getElementById('card' + index);
		cards[index] = b.value;
		localStorage.cards = JSON.stringify(cards);
	}
	
	// clears all cards and localStorage
	function reset() {
		localStorage.cards = JSON.stringify([]);
		cards = [];
	}
	
	// removes data for a card
	function delete_card(index) {
		cards.splice(index, 1); // removes data from array
		cards = cards; // refreshes the view
		localStorage.cards = JSON.stringify(cards); // updates localStorage
	}
	
	// borrowed function to shift items in array around
	function array_move(arr, old_index, new_index) {
		if (new_index >= arr.length) {
			var k = new_index - arr.length + 1;
			while (k--) {
				arr.push(undefined);
			}
		}
		arr.splice(new_index, 0, arr.splice(old_index, 1)[0]);
		return arr; // for testing
	}
	
	// called after a card is dragged and dropped
	function end_move(e, index) {
		const allCards = [...document.querySelectorAll('div[class~="popout"]')];
		var min = 0;
		const cx = e.clientX;
		const cy = e.clientY;
		// locates closest card to cursor
		for (const x in allCards) {
			const r = get_rect_center(allCards[x]);
			const m = get_rect_center(allCards[min]);
			if ((Math.abs(cx - r[0]) + Math.abs(cy - r[1]))/2 < (Math.abs(cx - m[0]) + Math.abs(cy - m[1]))) {
				min = x;
			}
		}
		array_move(cards, index, min);
		cards = cards;
		localStorage.cards = JSON.stringify(cards);
	}
	
	// returns center point of an element
	function get_rect_center(rect) {
		const r = rect.getBoundingClientRect();
		const rx = r['left'] + (r['width']/2);
		const ry = r['top'] + (r['height']/2);
		return [rx, ry];
	}
	
	function toggle_theme() {
		dark_mode = !dark_mode;
		localStorage.dark_mode = JSON.stringify(dark_mode);
		window.document.body.classList.toggle('dark-mode');
	}
	
</script>

<main id="main">
	<div class="center">
		<h1 transition:fade="{{duration: 4000}}">card wall</h1>
		<div class="board" id="board">
			{#each cards as card, index}
				<div class="popout" draggable="true" on:dragend|stopPropagation={e => end_move(e, index)} in:scale out:scale>
					<div class="controls">
						<button class="control" on:click={() => delete_card(index)}></button>
					</div>
					<textarea on:blur={() => update(index)} id="card{index}" class="cardtext">{card}</textarea>
				</div>
			{/each}
		</div>
		<button class="plus-button" on:click={add_card}>+</button>
		<button on:click={reset}>clear</button>
		<button on:click={() => localStorage.cards = JSON.stringify(cards)}>save</button>
		<button on:click={toggle_theme}>dark/light</button>
	</div>
</main>

<style>
	:global(body) {
		background-color: #ffeeee;
	}
	
	:global(body.dark-mode) {
		background-color: #5f6160;
	}
	
	.plus-button {
		color: green;
		padding: 6px 18px;
		background-color: #fa999f;
	}
	
	.center {
		text-align: center;
		justify-content: center;
	}
	
	.board {
		display: block;
		justify-content: center;
		min-height: 300px;
	}
	
	.controls {
		display: flex;
		justify-content: flex-end;
		margin-bottom: 0px;
	}
	
	.control {
		background-color: #fa999f;
		border-radius: 2px;
		padding: 3px 7px;
		margin-bottom: 2px;	
	}
	
	main {
		display: flex;
		justify-content: center;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		margin-bottom: 0px;
		margin-top: 0px;
	}
	
	.popout {
		background-color: white;
		height: 400px;
		width: 93%;
		padding: 10px;
		margin: 30px 0px;
		border-radius: 10px;
		box-shadow: 0 4px 8px 0 rgba(0,0,0,0.5);
		transition: transform 0.05s;
	}
	
	.popout:hover {
		transform: scale(1.01);
		box-shadow: 0 8px 16px 0 rgba(0,0,0,0.5);
	}
	
	.cardtext:focus {
		outline: none;
	}
	
	.cardtext {
		width: 100%;
		height: 90%;
		border: none;
		resize: none;
		padding: 0px;
		color: #ff3e00;
		font-size: 14px;
		font-family: arial;
	}
	
	@media (min-width: 359px) {
		.board {
			display: grid;
			grid-template-columns: 150px 150px;
			column-gap: 30px;
			row-gap: 20px;
			padding: 10px;
			min-height: 300px;
		}
		
		.popout {
			height: 200px;
			margin: 0px;
		}
		
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
		
		.board {
			display: grid;
			grid-template-columns: 150px 150px 150px;
			column-gap: 50px;
			row-gap: 20px;
			padding: 20px;
			min-height: 300px;
		}
		
		.popout {
			width: 150px;
		}
	}
</style>