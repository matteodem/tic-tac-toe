<script lang="ts" setup>
const pointsGrid = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])

const currentPlayer = ref('X')
const winner = ref('')

const clickPoint = (point: string, rowIndex: number, pointIndex: number) => {
  if (point !== '' || winner.value) {
    return null
  }

  pointsGrid.value[rowIndex][pointIndex] = currentPlayer.value

  if (currentPlayer.value === 'X') {
    currentPlayer.value = 'O'
  } else if (currentPlayer.value === 'O') {
    currentPlayer.value = 'X'
  }

  checkWinner()
}

/**
 * A map that consists of [rowIndex, pointIndex].
 * If the map matches with the same value, then there's a winner
 */
const threeInARowMap = [
  // horizontal
  [[0, 0], [0, 1], [0, 2]],
  [[1, 0], [1, 1], [1, 2]],
  [[2, 0], [2, 1], [2, 2]],
  // vertical
  [[0, 0], [1, 0], [2, 0]],
  [[0, 1], [1, 1], [2, 1]],
  [[0, 2], [1, 2], [2, 2]],
  // diagonal
  [[0, 0], [1, 1], [2, 2]],
  [[2, 0], [1, 1], [0, 2]],
]

const checkPlayerHasThreeInARow = (playerId: string) => {
  return threeInARowMap.some((map: any) => {
    return map.every((index: any) => {
      const value = pointsGrid.value[index[0]][index[1]]
      return value === playerId
    })
  })
}

const checkWinner = () => {
  if (checkPlayerHasThreeInARow('X')) {
    winner.value = 'X'
  } else if (checkPlayerHasThreeInARow('O')) {
    winner.value = 'O'
  }
}

</script>
<template>
  <ClientOnly>
    <h1>Tic Tac Toe</h1>

    <p v-if="winner">Player {{winner}} has won!</p>

    <div v-for="(row, rowIndex) in pointsGrid" class="point-row">
      <div v-for="(point, pointIndex) in row"
           class="point-item"
           @click="clickPoint(point, rowIndex, pointIndex)"
      >
        {{ point }}
      </div>
    </div>
  </ClientOnly>
</template>
<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;400;700&display=swap');

body {
  font-family: 'Roboto', sans-serif;
  margin: 0 auto;
  text-align: center;
  padding-top: 50px;
}

.point-row {
  display: flex;
  justify-content: center;
}

.point-item {
  width: 80px;
  height: 80px;
  border: 1px solid black;
  margin: 5px;
  padding: 5px;
  cursor: pointer;
  display: flex;
  user-select: none;
  justify-content: center;
  align-items: center;
  font-size: 30px;
}
</style>
