<script>
	function add_card() {
		var div = document.createElement('div');
		div.setAttribute('class', 'popout');
		document.getElementById('board').appendChild(div);
	}
</script>

<main id="main">
	<h1>card czar</h1>
	<div class="board" id="board">
	</div>
	<button class="plus-button" on:click={add_card}>+</button>
</main>

<style>
	:global(body) {
		background-color: white;
	}
	
	.plus-button {
		left: 20px;
		bottom: 10px;
		color: green;
		padding: 8px 20px;
		background-color: #fa999f;
	}
	
	.board {
		display: grid;
	}
	
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
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
		border-radius: 10px;
		box-shadow: 0 4px 8px 0 rgba(0,0,0,0.5);
		transition: transform 0.05s
	}
	
	.popout:hover {
		transform: scale(1.01);
		box-shadow: 0 8px 16px 0 rgba(0,0,0,0.5);
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>