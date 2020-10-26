<script>
	function add_card() {
		var div = document.createElement('div');
		div.setAttribute('class', 'popout');
		document.getElementById('board').appendChild(div);
	}
</script>

<main id="main">
	<div>
		<h1>card czar</h1>
		<div class="board" id="board"/>
		<button class="plus-button" on:click={add_card}>+</button>
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
		min-height: 400px;
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