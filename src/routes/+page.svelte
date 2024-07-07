<script lang="ts">
	import { onMount } from 'svelte';

	let selectedMineCount = 3;

	let grid: Array<{ mine: boolean; revealed: boolean }> = [];
	let gameOver: boolean;
	let message = '';

	function initializeGame(numberOfMines: number) {
		gameOver = false;
		grid = Array.from({ length: 25 }, () => ({ mine: false, revealed: false }));
		while (numberOfMines > 0) {
			const index = Math.floor(Math.random() * 24) + 1;

			if (!grid[index].mine) {
				grid[index].mine = true;
				numberOfMines--;
			}
		}
	}

	function handleClick(cell: { mine: boolean; revealed: boolean }) {
		if (gameOver) return;
		cell.revealed = true;
		if (cell.mine) {
			gameOver = true;
			message = 'Game Over';
		}

		grid = grid;
	}

	$: initializeGame(selectedMineCount);
</script>

<div class="p-10 py-20 max-w-[70%] min-h-[90%] bg-slate-800/90 backdrop-blur-sm flex rounded-xl">
	<section class="w-[35%] p-6 text-slate-400 rounded-md bg-slate-900 min-h-[90%]">
		<p class="text-sm">Number of mines</p>
		<div class="flex gap-2 py-2">
			{#each [3, 5, 10, 24] as count}
				<label
					class="rounded-sm text-slate-300 bg-slate-700 cursor-pointer w-20 py-2 flex items-center justify-center"
					class:selected={selectedMineCount == count}
				>
					<input class="hidden" type="radio" value={count} bind:group={selectedMineCount} />
					<span>{count}</span>
				</label>
			{/each}
		</div>
		{#if gameOver}
			<div class="mx-auto mt-10 text-center">
				<p class="text-sm pb-5">{message}</p>
				<button
					on:click={() => initializeGame(selectedMineCount)}
					class="bg-rose-500 py-2 px-6 rounded-sm text-white">Play Again</button
				>
			</div>
		{/if}
	</section>

	<section class="flex justify-center items-center mx-auto">
		<div class="grid grid-cols-5 gap-2.5">
			{#each grid as cell, index}
				<button
					class="backdrop-blur-sm drop-shadow-2xl h-20 w-20 text-2xl hover:bg-slate-600 bg-slate-700 flex items-center justify-center rounded-sm"
					class:wrong={cell.revealed && cell.mine}
					class:correct={cell.revealed && !cell.mine}
					on:click={() => handleClick(cell)}
				>
					{cell.revealed ? (cell.mine ? '💣' : '💎') : ''}
				</button>
			{/each}
		</div>
	</section>
</div>

<style>
	.selected {
		@apply bg-rose-500 text-white;
	}

	.correct {
		@apply bg-gradient-to-bl from-yellow-500/60 to-green-400/60;
	}

	.wrong {
		@apply bg-red-500;
	}
</style>
