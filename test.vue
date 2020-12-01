<template>
  <div>
    <span>Input table size: <input min="3" type="number" v-model.number="tblSize" v-on:blur="reCreateTable()" class="form-control"></span>
    <h1>{{ gameName }} ({{ tblSize }}X{{ tblSize }})</h1>
    <h1>Turn {{player}}</h1>
    <table>
      <tr v-for="row in tblSize" v-bind:key="row.id">
        <th v-for="col in tblSize" v-bind:key="col.id">
          <!-- <input class="box" v-on:click="updateScore(row, col)" v-model="player"> -->
          <button class="box" v-on:click="updateScore(row,col)"><input type="text" disabled v-model="items[row-1][col-1]"></button>
        </th>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  created() {
    this.items = [
      ['','',''],
      ['','',''],
      ['','','']
    ]
    // this.tblSize = 3;
    console.log(this.items);
  },
  watch: {

  },
  methods: {
    reCreateTable(){
      // for (let i=1;i<=this.tblSize;i++) {
      //   console.log(i);
      // }
    },updateScore(row, col){
      console.log(`(${row-1},${col-1})`);
      if (this.items[row-1][col-1] == '') {
        this.items[row-1][col-1] = this.player;
        if (this.player == 'X') {
          this.player = 'O'
        }else {
          this.player = 'X'
        }
      }
      
      console.log(this.items);
    }
  },
  data () {
    return {
      player: 'X',
      items: [],
      // tblSize: 0
    }
  },
  name: 'HelloWorld',
  // components: GameTable,
  props: {
    gameName: String,
    tblSize: {
      type:Number,
      default: 3
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box{
  width: 100px;
  height: 100px;
}
table{
  margin-left: auto;
  margin-right: auto;
}
.custom-table{
  margin-left: auto;
  margin-right: auto;
}
</style>
