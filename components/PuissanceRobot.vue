<template>
  <div class="min-h-screen bg-gray-600 p-8">
    <div class="">
      <div class="board">
        <div
          v-for="(row, rowIndex) in board"
          :key="rowIndex"
          class="row"
        >
          <div
            v-for="(cell, cellIndex) in row"
            :key="cellIndex"
            @click="play(cellIndex)"
            class="cell"
            :class="{
            'cell-red': cell === 1,
            'cell-yellow': cell === 2,
            'cell-win': winningCells.includes(rowIndex * 7 + cellIndex)
          }"
          ></div>
        </div>
      </div>
      <div v-if="winner" class="text-white text-center message">
        {{ winner }} a gagné !
        <button @click="restart">Rejouer</button>
      </div>
      <div v-else-if="isBoardFull" class="text-white text-center message">
        Match nul !
        <button @click="restart">Rejouer</button>
      </div>
      <div v-else class="text-white text-center message">{{ currentPlayer }} à toi de jouer</div>
    </div>


    <div class="text-center mb-4">
      <h3 class="text-3xl font-extrabold leading-none tracking-tight dark:text-white">
        Tu veux jouer contre ton <span class="text-transparent bg-clip-text bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100"> pote </span> ?
      </h3>
      <br>

      <nuxt-link class="w-full mb-10 text-gray-900 bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100 dark:focus:ring-red-400 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2" to="/normal">Clique ici</nuxt-link>

    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPlayer: 1,
      board: Array(6)
        .fill()
        .map(() => Array(7).fill(0)),
      winningCells: [],
      winner: null
    }
  },
  computed: {
    isBoardFull() {
      return this.board.every(row => row.every(cell => cell !== 0))
    }
  },
  methods: {
    play(columnIndex) {
      if (this.winner || this.isBoardFull) return

      const rowIndex = this.getNextEmptyRow(columnIndex)
      if (rowIndex === null) return

      this.$set(this.board[rowIndex], columnIndex, this.currentPlayer)
      this.checkForWin(rowIndex, columnIndex)
      this.currentPlayer = this.currentPlayer === 1 ? 2 : 1

      // Play robot move
      if (!this.winner && !this.isBoardFull && this.currentPlayer === 2) {
        const robotMove = this.getRobotMove()
        setTimeout(() => {
          this.play(robotMove)
        }, 500)
      }
    },
    getNextEmptyRow(columnIndex) {
      for (let rowIndex = 5; rowIndex >= 0; rowIndex--) {
        if (this.board[rowIndex][columnIndex] === 0) {
          return rowIndex
        }
      }
      return null
    },
    checkForWin(rowIndex, columnIndex) {
      const player = this.board[rowIndex][columnIndex]
      const directions = [
        {row: 1, col: 0},
        {row: 0, col: 1},
        {row: 1, col: 1},
        {row: -1, col: 1}
      ]
      for (const direction of directions) {
        const count1 = this.countCells(rowIndex, columnIndex, direction)
        const count2 = this.countCells(rowIndex, columnIndex, {row: -direction.row, col: -direction.col})
        if (count1 + count2 - 1 >= 4) {
          this.winner = player === 1 ? 'Rouge' : 'Jaune'
          this.highlightWinningCells(rowIndex, columnIndex, direction)
          return
        }
      }
    },

    countCells(rowIndex, columnIndex, direction) {
      let row = rowIndex
      let col = columnIndex
      let count = 0
      while (
        row >= 0 &&
        row < 6 &&
        col >= 0 &&
        col < 7 &&
        this.board[row][col] === this.currentPlayer
        ) {
        row += direction.row
        col += direction.col
        count++
      }
      return count
    },

    getRobotMove() {
      const availableColumns = []
      for (let columnIndex = 0; columnIndex < 7; columnIndex++) {
        const rowIndex = this.getNextEmptyRow(columnIndex)
        if (rowIndex !== null) {
          availableColumns.push(columnIndex)
        }
      }

      // Check for immediate winning moves
      for (const columnIndex of availableColumns) {
        const rowIndex = this.getNextEmptyRow(columnIndex)
        this.$set(this.board[rowIndex], columnIndex, 2)
        if (this.checkForWin(rowIndex, columnIndex)) {
          this.$set(this.board[rowIndex], columnIndex, 0)
          return columnIndex
        }
        this.$set(this.board[rowIndex], columnIndex, 0)
      }

      // Check for immediate losing moves
      for (const columnIndex of availableColumns) {
        const rowIndex = this.getNextEmptyRow(columnIndex)
        this.$set(this.board[rowIndex], columnIndex, 1)
        if (this.checkForWin(rowIndex, columnIndex)) {
          this.$set(this.board[rowIndex], columnIndex, 0)
          return columnIndex
        }
        this.$set(this.board[rowIndex], columnIndex, 0)
      }

      // Choose a random available column
      return availableColumns[Math.floor(Math.random() * availableColumns.length)]
    },


    highlightWinningCells(rowIndex, columnIndex, direction) {
      const cells = []
      let row = rowIndex
      let col = columnIndex
      while (
        row >= 0 &&
        row < 6 &&
        col >= 0 &&
        col < 7 &&
        this.board[row][col] === this.currentPlayer
        ) {
        cells.push(row * 7 + col)
        row += direction.row
        col += direction.col
      }
      if (cells.length >= 4) {
        this.winningCells = cells
      } else {
        this.winningCells = []
      }
    },
    restart() {
      this.currentPlayer = 1
      this.board = Array(6)
        .fill()
        .map(() => Array(7).fill(0))
      this.winningCells = []
      this.winner = null

      // Add robot code here
    }
  }
}
</script>

<style scoped>
.board {
  background: #202078;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
  max-width: 500px;
}

.row {
  display: flex;
}

.cell {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background-color: #fff;
  border: 1px solid #ccc;
  margin: 5px;
  cursor: pointer;
}

.cell-red {
  background-color: #ff4136;
}


.cell-yellow {
  transition: width 5s linear;
  background-color: #ffdc00;
}


.cell-win {
  background-color: #fff;
  border-color: #ff4136;
}

.message {
  font-size: 24px;
  margin-bottom: 10px;
}
</style>
