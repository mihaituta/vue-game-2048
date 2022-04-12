<template>
  <Score :score="score" @newGame="newGame" />
  <Result :result="result" />

  <div id="board" ref="boardRef">
    <Tile :boardTiles="boardTiles" />
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import Score from './Score'
import Result from './Result'
import Tile from './Tile'

const boardTiles = ref([])
const score = ref(0)
const result = ref('')
const size = ref(4)

let emptyTiles = computed(() => boardTiles.value.filter((tile) => tile === 0))

const addRandomTile = () => {
  const randomIndex = Math.floor(Math.random() * emptyTiles.value.length)
  // console.log(randomIndex)
  if (boardTiles.value[randomIndex] === 0) {
    boardTiles.value[randomIndex] = Math.random() > 0.8 ? 4 : 2
    checkForGameOver()
  } else {
    addRandomTile()
  }
  // console.log(emptyTiles.value)
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

const slideTiles = (direction, i) => {
  const val = direction === 'left' || direction === 'right' ? 1 : size.value

  let tiles = new Array(4)
  tiles[0] = boardTiles.value[i]
  tiles[1] = boardTiles.value[i + val]
  tiles[2] = boardTiles.value[i + val * 2]
  tiles[3] = boardTiles.value[i + val * 3]

  let line = [tiles[0], tiles[1], tiles[2], tiles[3]]

  let filteredLine = line.filter((num) => num)
  let missing = size.value - filteredLine.length
  let zeros = Array(missing).fill(0)
  let newLine =
    direction === 'up' || direction === 'left'
      ? filteredLine.concat(zeros)
      : zeros.concat(filteredLine)

  boardTiles.value[i] = newLine[0]
  boardTiles.value[i + val] = newLine[1]
  boardTiles.value[i + val * 2] = newLine[2]
  boardTiles.value[i + val * 3] = newLine[3]
}

const moveVertical = (direction) => {
  for (let i = 0; i < 4; i++) slideTiles(direction, i)
}

const moveHorizontal = (direction) => {
  for (let i = 0; i < size.value * size.value; i++)
    if (i % size.value === 0) slideTiles(direction, i)
}

const combineTiles = (direction) => {
  const length = direction === 'row' ? 15 : 12
  const val = direction === 'row' ? 1 : size.value
  for (let i = 0; i < length; i++) {
    if (boardTiles.value[i] === boardTiles.value[i + val]) {
      let combinedTotal = boardTiles.value[i] + boardTiles.value[i + val]
      boardTiles.value[i] = combinedTotal
      boardTiles.value[i + val] = 0
      score.value += combinedTotal
    }
  }
  resultCheck()
}

const setupInput = () => window.addEventListener('keyup', handleInput)

const handleInput = (e) => {
  switch (e.key) {
    case 'ArrowUp':
      moveVertical('up')
      combineTiles('column')
      moveVertical('up')
      addRandomTile()
      break
    case 'ArrowDown':
      moveVertical('down')
      combineTiles('column')
      moveVertical('down')
      addRandomTile()
      break
    case 'ArrowLeft':
      moveHorizontal('left')
      combineTiles('row')
      moveHorizontal('left')
      addRandomTile()
      break
    case 'ArrowRight':
      moveHorizontal('right')
      combineTiles('row')
      moveHorizontal('right')
      addRandomTile()
      break
  }
}

const resultCheck = () => {
  for (let i = 0; i < boardTiles.value.length; i++) {
    if (boardTiles.value[i] === 2048) {
      result.value = 'You won!'
      window.removeEventListener('keyup', handleInput)
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
    result.value = 'Game Over'
    window.removeEventListener('keyup', handleInput)
  }
}

onMounted(() => {
  setupInput()
  createBoard()
})
</script>

<style scoped lang="scss">
#board {
  width: 400px;
  height: 400px;
  background-color: hsl(0, 0%, 30%);
  border: 5px solid hsl(0, 0%, 30%);
  border-radius: 0.3rem;
  display: flex;
  flex-wrap: wrap;
}
</style>
