<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])

const CalculateWinner = (board) => {
  const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }

  return null
}

const winner = computed(() => CalculateWinner(board.value.flat()))

const MakeMove = (x, y) => {
	if (winner.value) return

	if (board.value[x][y]) return

	board.value[x][y] = player.value

	player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
	board.value = [
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	player.value = 'X'
}

</script>

<template>
	<main class="pt-8 text-center">
	  <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
  
	  <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>
  
	  <div class="flex flex-col items-center mb-8">
		<div
		  v-for="(row, x) in board"
		  :key="x"
		  class="flex"
		>
		  <div
			v-for="(cell, y) in row"
			:key="y"
			@click="MakeMove(x, y)"
			class="cell"
		  >
			{{ cell === 'X' ? 'x' : cell === 'O' ? 'o' : '' }}
		  </div>
		</div>
	  </div>
  
	  <div class="text-center">
		<h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
		<button @click="ResetGame" class="button">Restart</button>
	  </div>
	</main>
  </template>

<style>
.cell {
  width: 80px;
  height: 80px;
  border: 2px solid #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2em;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.cell:hover {
  background-color: #f0f0f0;
}

.button {
  padding: 10px 20px;
  background-color: cornflowerblue;
  color: #fff;
  border: none;
  border-radius: 5px;
  font-size: 1em;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: lightskyblue;
}
</style>
