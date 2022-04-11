<template>
  <Score :score="score" @newGame="newGame" />

  <h2 class="result" v-if="result !== ''">{{ result }}</h2>

  <div id="board" ref="boardRef">
    <div v-for="tile in boardTiles" :class="`tile x${tile}`">
      <div v-show="tile !== 0">{{ tile }}</div>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import Score from './Score.vue'

const boardTiles = ref([])
const score = ref(0)
const result = ref('')
const size = ref(4)

let emptyTiles = computed(() => boardTiles.value.filter((tile) => tile === 0))

const addRandomTile = () => {
  const randomIndex = Math.floor(Math.random() * emptyTiles.value.length)
  console.log(randomIndex)
  if (boardTiles.value[randomIndex] === 0) {
    boardTiles.value[randomIndex] = Math.random() > 0.8 ? 4 : 2
    checkForGameOver()
  } else {
    addRandomTile()
  }
  console.log(emptyTiles.value)
}

const newGame = () => {
  boardTiles.value = []
  result.value = ''
  score.value = 0
  setupInput()
  createBoard()
}

const createBoard = () => {
  for (let i = 0; i < size.value * size.value; i++) {
    boardTiles.value.push(0)
  }

  addRandomTile()
  addRandomTile()
}

const moveUp = () => {
  for (let i = 0; i < 4; i++) {
    let one = boardTiles.value[i]
    let two = boardTiles.value[i + size.value]
    let three = boardTiles.value[i + size.value * 2]
    let four = boardTiles.value[i + size.value * 3]
    let column = [one, two, three, four]

    let filteredColumn = column.filter((num) => num)
    let missing = size.value - filteredColumn.length
    let zeros = Array(missing).fill(0)
    let newColumn = filteredColumn.concat(zeros)

    boardTiles.value[i] = newColumn[0]
    boardTiles.value[i + size.value] = newColumn[1]
    boardTiles.value[i + size.value * 2] = newColumn[2]
    boardTiles.value[i + size.value * 3] = newColumn[3]
  }
}

const moveDown = () => {
  for (let i = 0; i < 4; i++) {
    let one = boardTiles.value[i]
    let two = boardTiles.value[i + size.value]
    let three = boardTiles.value[i + size.value * 2]
    let four = boardTiles.value[i + size.value * 3]
    let column = [one, two, three, four]

    let filteredColumn = column.filter((num) => num)
    let missing = size.value - filteredColumn.length
    let zeros = Array(missing).fill(0)
    let newColumn = zeros.concat(filteredColumn)

    boardTiles.value[i] = newColumn[0]
    boardTiles.value[i + size.value] = newColumn[1]
    boardTiles.value[i + size.value * 2] = newColumn[2]
    boardTiles.value[i + size.value * 3] = newColumn[3]
  }
}

const moveRight = () => {
  for (let i = 0; i < size.value * size.value; i++) {
    if (i % size.value === 0) {
      let one = boardTiles.value[i]
      let two = boardTiles.value[i + 1]
      let three = boardTiles.value[i + 2]
      let four = boardTiles.value[i + 3]

      let row = [one, two, three, four]

      let filteredRow = row.filter((num) => num)
      let missing = size.value - filteredRow.length
      let zeros = Array(missing).fill(0)
      let newRow = zeros.concat(filteredRow)

      boardTiles.value[i] = newRow[0]
      boardTiles.value[i + 1] = newRow[1]
      boardTiles.value[i + 2] = newRow[2]
      boardTiles.value[i + 3] = newRow[3]
    }
  }
}

const moveLeft = () => {
  for (let i = 0; i < size.value * size.value; i++) {
    if (i % size.value === 0) {
      let one = boardTiles.value[i]
      let two = boardTiles.value[i + 1]
      let three = boardTiles.value[i + 2]
      let four = boardTiles.value[i + 3]

      let row = [one, two, three, four]

      let filteredRow = row.filter((num) => num)
      let missing = size.value - filteredRow.length
      let zeros = Array(missing).fill(0)
      let newRow = filteredRow.concat(zeros)

      boardTiles.value[i] = newRow[0]
      boardTiles.value[i + 1] = newRow[1]
      boardTiles.value[i + 2] = newRow[2]
      boardTiles.value[i + 3] = newRow[3]
    }
  }
}

const combineRow = () => {
  for (let i = 0; i < 15; i++) {
    if (boardTiles.value[i] === boardTiles.value[i + 1]) {
      let combinedTotal = boardTiles.value[i] + boardTiles.value[i + 1]
      boardTiles.value[i] = combinedTotal
      boardTiles.value[i + 1] = 0
      score.value += combinedTotal
    }
  }
  resultCheck()
}

const combineColumn = () => {
  for (let i = 0; i < 12; i++) {
    if (boardTiles.value[i] === boardTiles.value[i + size.value]) {
      let combinedTotal = boardTiles.value[i] + boardTiles.value[i + size.value]
      boardTiles.value[i] = combinedTotal
      boardTiles.value[i + size.value] = 0
      score.value += combinedTotal
    }
  }
  resultCheck()
}

const setupInput = () =>
  document.addEventListener('keyup', handleInput, { once: true })

const handleInput = (e) => {
  switch (e.key) {
    case 'ArrowUp':
      moveUp()
      combineColumn()
      moveUp()
      addRandomTile()
      break
    case 'ArrowDown':
      moveDown()
      combineColumn()
      moveDown()
      addRandomTile()
      break
    case 'ArrowLeft':
      moveLeft()
      combineRow()
      moveLeft()
      addRandomTile()
      break
    case 'ArrowRight':
      moveRight()
      combineRow()
      moveRight()
      addRandomTile()
      break
    default:
      setupInput()
      return
  }
  setupInput()
}

const resultCheck = () => {
  for (let i = 0; i < boardTiles.value.length; i++) {
    if (boardTiles.value[i] === 2048) {
      result.value = 'You won!'
      document.removeEventListener('keyup', handleInput)
    }
  }
}

const checkForGameOver = () => {
  let zeros = 0
  for (let i = 0; i < boardTiles.value.length; i++) {
    if (boardTiles.value[i] === 0) {
      zeros++
    }
  }
  if (zeros === 0) {
    result.value = 'You lost!'
    document.removeEventListener('keyup', handleInput)
  }
}

onMounted(() => {
  setupInput()
  createBoard()
})
</script>

<style scoped lang="scss">
.result {
  margin-bottom: 1rem;
  font-size: 2rem;
}

#board {
  width: 400px;
  height: 400px;
  background-color: hsl(0, 0%, 30%);
  border: 5px solid hsl(0, 0%, 30%);
  border-radius: 0.3rem;
  display: flex;
  flex-wrap: wrap;

  .tile {
    width: 90px;
    height: 90px;
    background-color: hsl(0, 0%, 49%);
    border: 5px solid hsl(0, 0%, 30%);
    border-radius: 0.3rem;

    font-size: 2rem;
    font-weight: bold;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .x2 {
    background-color: hsl(152, 47%, 80%);
    color: hsl(0, 0%, 52%);
  }

  .x4 {
    background-color: hsl(162, 32%, 52%);
    color: hsl(0, 0%, 100%);
  }

  .x8 {
    background-color: hsl(156, 53%, 45%);
    color: hsl(0, 0%, 100%);
  }

  .x16 {
    background-color: hsl(154, 100%, 41%);
    color: hsl(0, 0%, 100%);
  }

  .x32 {
    background-color: hsl(131, 52%, 40%);
    color: hsl(0, 0%, 100%);
  }

  .x64 {
    background-color: hsl(130, 90%, 41%);
    color: hsl(0, 0%, 98%);
  }

  .x128 {
    background-color: hsl(179, 47%, 80%);
    color: hsl(0, 0%, 52%);
  }

  .x256 {
    background-color: hsl(179, 40%, 61%);
    color: hsl(0, 0%, 100%);
  }

  .x512 {
    background-color: hsl(181, 49%, 43%);
    color: hsl(0, 0%, 100%);
  }

  .x1024 {
    background-color: hsl(181, 100%, 58%);
    color: hsl(0, 0%, 52%);
  }

  .x2048 {
    background-color: hsl(358, 85%, 63%);
    color: hsl(0, 0%, 100%);
  }
}
</style>
