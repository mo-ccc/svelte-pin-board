<script>
	console.log(localStorage.cards);
	var cards = JSON.parse(localStorage.cards);
	var held;
	
	function add_card() {
		cards.push("");
		cards[0] = cards[0];
		console.log(cards);
	}
	function update(index) {
		const b = document.getElementById('card' + index);
		cards[index] = b.value;
		console.log(cards);
		localStorage.cards = JSON.stringify(cards);
		console.log("local", localStorage.cards);
	}
	
	function reset() {
		localStorage.cards = JSON.stringify([]);
		cards = [];
	}
	
	function delete_card(index) {
		console.log('delete', index);
		cards.splice(index, 1);
		cards[0] = cards[0];
		localStorage.cards = JSON.stringify(cards);
	}
	
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
	
	function end_move(e, index) {
		const allElem = [...document.querySelectorAll('div[class~="popout"]')];
		// allElem.splice(index, 1);
		var min = 0;
		const cx = e.clientX;
		const cy = e.clientY;
		for (const x in allElem) {
			const r = getRectInfo(allElem[x]);
			const m = getRectInfo(allElem[min]);
			if ((Math.abs(cx - r[0]) + Math.abs(cy - r[1]))/2 < (Math.abs(cx - m[0]) + Math.abs(cy - m[1]))) {
				min = x;
			}
		}
		array_move(cards, index, min);
		cards = cards;
	}
	
	function getRectInfo(rect) {
		const r = rect.getBoundingClientRect();
		const rx = r['left'] + (r['width']/2);
		const ry = r['top'] + (r['height']/2);
		return [rx, ry];
	}
	
</script>

<main id="main">
	<div>
		<h1>card wall</h1>
		<div class="board" id="board">
			{#each cards as card, index}
				<div class="popout" draggable="true" on:dragend|stopPropagation={e => end_move(e, index)}>
					<div class="controls">
						<button class="control" on:click={() => delete_card(index)}></button>
					</div>
					<textarea on:blur={() => update(index)} id="card{index}" class="cardtext">{card}</textarea>
				</div>
			{/each}
		</div>
		<button class="plus-button" on:click={add_card}>+</button>
		<button on:click={reset}>clear</button>
	</div>
</main>

<style>
	:global(body) {
		background-color: white;
	}
	
	.plus-button {
		color: green;
		padding: 8px 20px;
		background-color: #fa999f;
	}
	
	.board {
		display: grid;
		grid-template-columns: 150px 150px 150px;
		column-gap: 50px;
		row-gap: 20px;
		padding: 40px;
		min-height: 300px;
	}
	
	.moving {
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
	}
	
	:global(.popout) {
		background-color: white;
		height: 200px;
		width: 150px;
		padding: 10px;
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

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>