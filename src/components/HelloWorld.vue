<template>
  <div>
    <span>Input size: <input min="3" type="number" v-model.number="tblSize" class="form-control"></span>
    <button v-on:click="reCreateTable()">Generate</button>
    <h1>{{ gameName }} ({{ tblSize }}X{{ tblSize }})</h1>
    <h1>Turn {{player}}</h1>
    <table>
      <tr v-for="row in tblSizeDisplay" v-bind:key="row.id">
        <th v-for="col in tblSizeDisplay" v-bind:key="col.id">
          <button class="box" v-on:click="updateScore(row,col)"><input class="box-label" type="text" disabled v-model="board[row-1][col-1]"></button>
        </th>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  created() {
    this.board = [
      [null,null,null],
      [null,null,null],
      [null,null,null]
    ]
  },
  watch: {

  },
  methods: {
    reCreateTable(){
      this.tblSizeDisplay = this.tblSize;
      this.board = [];
      for (let i=0; i<this.tblSize;i++){
        let inside = [];
        for (let j=0; j<this.tblSize;j++) {
          inside.push(null);
        }
        this.board.push(inside);
      }
      this.player = 'X'
    },
    updateScore(row, col){
      if (this.board[row-1][col-1] == null) {
        this.board[row-1][col-1] = this.player;
        if (this.player == 'X') {
          this.player = 'O'
        }else {
          this.player = 'X'
        }
      }
      this.checkWinner(row,col);
    },checkWinner(row,col){
      let player = this.board[row-1][col-1]
      if (this.checkAround(row,col, player) >= (this.tblSizeDisplay-1)) {
        alert(`${player} WON THE GAME!!!!`)
      }
    },checkAround(row,col,player){
      row = row-1
      col = col-1
      let countResult = 0;
      let listResult = [];
      let countVertical = 0;
      let countHorizontal = 0;
      let diagonalTopLeft = 0;
      let diagonalTopRight = 0;
      //  แนวตั้ง
      countVertical = countVertical + (this.checkTop(row,col,0,player)-1)
      countVertical = countVertical + (this.checkBottom(row,col,0,player)-1)
      //  แนวนอน
      countHorizontal = countHorizontal + (this.checkLeft(row,col,0,player)-1)
      countHorizontal = countHorizontal + (this.checkRight(row,col,0,player)-1)
      //  แนวทะแยง ล่างขวาไปซ้าย
      diagonalTopLeft = diagonalTopLeft + (this.checkTopLeft(row,col,0,player)-1)
      diagonalTopLeft = diagonalTopLeft + (this.checkBottomRight(row,col,0,player)-1)
      //  แนวทะแยงล่างซ้ายไปขวา
      diagonalTopRight = diagonalTopRight + (this.checkBottomLeft(row,col,0,player)-1)
      diagonalTopRight = diagonalTopRight + (this.checkTopRight(row,col,0,player)-1)

      listResult.push(countVertical)
      listResult.push(countHorizontal)
      listResult.push(diagonalTopLeft)
      listResult.push(diagonalTopRight)

      countResult = Math.max(...listResult);
      console.log(countResult);
      return countResult;
    },checkTop(row,col,countResult,player) {
      if (row >= 0 && this.board[row][col] == player) {
        countResult+=1;
        return this.checkTop(row-1,col,countResult,player)
      }else {
        return countResult;
      }
    },checkBottom(row,col,countResult,player) {
      if (row < this.tblSizeDisplay && this.board[row][col] == player) {
        countResult+=1;
        return this.checkBottom(row+1,col,countResult,player)
      }else {
        return countResult;
      }
    },checkLeft(row,col,countResult,player) {
      if (col >= 0 && this.board[row][col] == player) {
        countResult+=1;
        return this.checkLeft(row,col-1,countResult,player)
      }else {
        return countResult;
      }
    },checkRight(row,col,countResult,player) {
      if (col < this.tblSizeDisplay && this.board[row][col] == player) {
        countResult+=1;
        return this.checkRight(row,col+1,countResult,player)
      }else {
        return countResult;
      }
    },checkTopLeft(row,col,countResult,player) {
      if (row >= 0 && col >= 0 && this.board[row][col] == player) {
        countResult+=1;
        return this.checkTopLeft(row-1,col-1,countResult,player)
      }else {
        return countResult;
      }
    },checkBottomRight(row,col,countResult,player) {
      if (row < this.tblSizeDisplay && col < this.tblSizeDisplay && this.board[row][col] == player) {
        countResult+=1;
        return this.checkBottomRight(row+1,col+1,countResult,player)
      }else {
        return countResult;
      }
    },checkBottomLeft(row,col,countResult,player) {
      if (row < this.tblSizeDisplay && col >= 0 && this.board[row][col] == player) {
        countResult+=1;
        return this.checkBottomLeft(row+1,col-1,countResult,player)
      }else {
        return countResult;
      }
    },checkTopRight(row,col,countResult,player) {
      if (row >= 0 && col < this.tblSizeDisplay && this.board[row][col] == player) {
        countResult+=1;
        return this.checkTopRight(row-1,col+1,countResult,player)
      }else {
        return countResult;
      }
    }

  },
  data () {
    return {
      player: 'X',
      board: [],
    }
  },
  name: 'HelloWorld',
  props: {
    gameName: String,
    tblSize: {
      type:Number,
      default: 3
    },tblSizeDisplay: {
      type:Number,
      default: 3
    }
  }
}
</script>

<style scoped>
.box{
  width: 100px;
  height: 100px;
}
.box-label{
  border:none;
  width: 100px;
  height: 100px;
  font-size: 36px;
  text-align: center;
  margin-left: -10px;
}
table{
  margin-left: auto;
  margin-right: auto;
}
</style>