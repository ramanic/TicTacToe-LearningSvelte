<script>
	import { page } from '$app/stores';
	import { afterUpdate } from 'svelte';
	let count = 0;
	let gamestate = [
		[0, 0, 0],
		[0, 0, 0],
		[0, 0, 0]
	];
	let turn = 1;
	let winner = 0;

	// @ts-ignore

	const onClicked = (row, col) => {
		let ngamestate = gamestate;
		if (gamestate[row][col] === 0 && winner === 0) {
			ngamestate[row][col] = turn;
			turn = turn === 1 ? 2 : 1;
		}
		gamestate = [...ngamestate];
		winner = checkGameOver(gamestate);
	};
	const reset = () => {
		turn = 1;
		winner = 0;
		let ngamestate = [
			[0, 0, 0],
			[0, 0, 0],
			[0, 0, 0]
		];
		gamestate = [...ngamestate];
	};
	// @ts-ignore

	const add = () => {
		count += 1;
	};
	// @ts-ignore
	function checkGameOver(gamestate) {
		// check rows
		for (let i = 0; i < 3; i++) {
			if (
				gamestate[i][0] === gamestate[i][1] &&
				gamestate[i][0] === gamestate[i][2] &&
				gamestate[i][0] !== 0
			) {
				return gamestate[i][0];
			}
		}

		// check columns
		for (let i = 0; i < 3; i++) {
			if (
				gamestate[0][i] === gamestate[1][i] &&
				gamestate[0][i] === gamestate[2][i] &&
				gamestate[0][i] !== 0
			) {
				return gamestate[0][i];
			}
		}

		// check diagonals
		if (
			gamestate[0][0] === gamestate[1][1] &&
			gamestate[0][0] === gamestate[2][2] &&
			gamestate[0][0] !== 0
		) {
			return gamestate[0][0];
		}

		if (
			gamestate[0][2] === gamestate[1][1] &&
			gamestate[0][2] === gamestate[2][0] &&
			gamestate[0][2] !== 0
		) {
			return gamestate[0][2];
		}

		// check for draw
		for (let i = 0; i < 3; i++) {
			for (let j = 0; j < 3; j++) {
				if (gamestate[i][j] === 0) {
					return 0;
				}
			}
		}
		return -1;
	}

	// @ts-ignore
</script>

<div>
	<p class={winner != 0 ? 'over' : ''}>
		{winner == 1
			? 'Player X won'
			: winner == 2
			? 'Player O won'
			: winner == -1
			? 'Draw'
			: `Player ${turn == 1 ? 'X' : 'O'} turn`}
	</p>
	<p class="try" on:click={reset}>{winner != 0 ? 'Try Again' : ''}</p>
	<table>
		{#each gamestate as rows, row}
			<tr>
				{#each rows as item, col}
					<!-- svelte-ignore a11y-click-events-have-key-events -->
					<td
						on:click={() => {
							onClicked(row, col);
						}}
					>
						{gamestate[row][col] === 1 ? 'X' : gamestate[row][col] == 2 ? 'O' : ''}
					</td>
				{/each}
			</tr>{/each}
	</table>
</div>

<style>
	td:nth-child(1) {
		border-right: 5px solid black;
	}
	td:nth-child(2) {
		border-right: 5px solid black;
	}
	tr:nth-child(1) {
		border-bottom: 5px solid black;
	}
	tr:nth-child(2) {
		border-bottom: 5px solid black;
	}

	td {
		text-align: center;
		min-width: 200px;
		min-height: 200px;
		height: 200px;
		width: 200px;
		font-size: 5rem;
	}
	table {
		border-collapse: collapse;
	}
	p {
		font-size: 2rem;
		text-align: center;
		margin: 2rem;
	}
	.over {
		color: 'red';
	}
	.try {
		cursor: pointer;
	}
</style>
