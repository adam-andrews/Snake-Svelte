<script>
	// Setting up board and snake
	let snake = [];
	const rows = Array(9).fill('square');
	const squares = [];
	let direction = 'e';
	for (let i = 0; i <= 8; i++) {
		squares.push([...rows]);
	}
	// Adding snake
	squares[5][4] = 'food';

	snake.push([2, 5]);

	function updateBoard() {
		for (let i = 0; i <= 8; i++) {
			for (let j = 0; j <= 8; j++) {
				const currentSquare = squares[i][j] === 'food' ? 'food' : 'square';
				squares[i][j] = currentSquare;
			}
		}
		snake.forEach((coordinate) => {
			squares[coordinate[0]][coordinate[1]] = 'snake';
		});
	}

	function moveSnake() {
		let newPosition = [];
		if (direction === 'n') {
			newPosition = [snake[0][0] - 1, snake[0][1]];
		}
		if (direction === 'e') {
			newPosition = [snake[0][0], snake[0][1] + 1];
		}
		if (direction === 's') {
			newPosition = [snake[0][0] + 1, snake[0][1]];
		}
		if (direction === 'w') {
			newPosition = [snake[0][0], snake[0][1] - 1];
		}

		if (
			newPosition[0] > 8 ||
			newPosition[0] < 0 ||
			newPosition[1] > 8 ||
			newPosition[1] < 0 ||
			squares[newPosition[0]][newPosition[1]] === 'snake'
		) {
			gameOver();
			return;
		}

		snake = [newPosition, ...snake];
		console.log(squares[newPosition[0]][newPosition[1]]);
		if (squares[newPosition[0]][newPosition[1]] !== 'food') {
			snake.pop();
		} else {
			generateFood();
		}

		updateBoard();
	}
	function onKeyDown(e) {
		switch (e.keyCode) {
			case 38:
				if (squares[snake[0][0] - 1][snake[0][1]] !== 'snake') {
					direction = 'n';
				}
				break;
			case 40:
				if (squares[snake[0][0] + 1][snake[0][1]] !== 'snake') {
					direction = 's';
				}
				break;
			case 37:
				if (squares[snake[0][0]][snake[0][1] - 1] !== 'snake') {
					direction = 'w';
				}
				break;
			case 39:
				if (squares[snake[0][0]][snake[0][1] + 1] !== 'snake') {
					direction = 'e';
				}
				break;
		}
	}

	function generateFood() {
		const foodSquare = [
			Math.floor(Math.random() * 9),
			Math.floor(Math.random() * 9),
		];

		console.log('foodSquare', foodSquare);
		console.log('square', squares[foodSquare[0]][foodSquare[1]]);

		if (squares[foodSquare[0]][foodSquare[1]] === 'square') {
			squares[foodSquare[0]][foodSquare[1]] = 'food';
		} else {
			generateFood();
		}
	}

	function gameOver() {
		for (let i = 0; i <= 8; i++) {
			for (let j = 0; j <= 8; j++) {
				squares[i][j] = 'food';
			}
		}
	}

	setInterval(moveSnake, 100);
</script>

{direction}
<div class="board">
	{#each squares as row}
		{#each row as block}
			<div class={block} />
		{/each}
	{/each}
</div>
<svelte:window on:keydown|preventDefault={onKeyDown} />

<style>
	.board {
		background-color: black;
		display: grid;
		grid-template-columns: repeat(9, 45px);
		justify-content: center;
	}
	.square {
		width: 40px;
		height: 40px;
		background-color: red;
		margin: 5px;
	}

	.snake {
		width: 40px;
		height: 40px;
		background-color: blue;
		margin: 5px;
	}

	.food {
		width: 40px;
		height: 40px;
		background-color: yellow;
		margin: 5px;
	}
</style>
