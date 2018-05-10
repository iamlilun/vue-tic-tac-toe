<template>
<div class="app">
  <div class="board">
    <div class="grid"
      v-for="(grid, idx) in grids"
      @click="setGrid(idx)"
    >
      {{setSymbol(grid)}}
    </div>
  </div>
  <div class="info">
    <span>Player: {{setSymbol(player)}}</span>
    <br/>
    <span>Winner: {{setSymbol(winner)}}</span>
    <br/>
    <button @click="reset">Reset</button>
  </div>
</div>
</template>

<script>
const lines = [
    [0,1,2], [3,4,5], [6,7,8],
    [0,3,6], [1,4,7], [2,5,8],
    [0,4,8], [2,4,6]
];
export default {
    data(){
        return {
            player: 1,
            grids: [ //-1是x, 0是空白, 1是o
                0, 0, 0,
                0, 0, 0,
                0, 0, 0,
           ],
        };
    },
    computed:{
        winner(){
            return lines.reduce((winner, [a, b, c])=>{ //高階用reduce
                if(winner !== 0) return winner;
                const sum = this.grids[a] + this.grids[b] + this.grids[c];
                if(sum === 3) return 1;
                if(sum === -3) return -1;
                return 0;
            }, 0)
        },
    },
    methods:{
        setGrid(idx){
            if(this.grids[idx] !== 0) return; //只有空白格子才會切換值
           //this.grids[idx] = 1; <==直接指定idx.值是不會被更新的
           //Vue.set(this.grids, idx, 1); //可用Vue.set指定
            if(this.winner !== 0) return;
           this.$set(this.grids, idx, this.player); //也可用this.$set指定
           this.player = -this.player;
           //this.winner = this.getWinner(); //移到computed後就不需要再手動指定

        },
        setSymbol(num){
            return num === 0? '' : num === 1? 'o' : 'x';
        },
        // getWinner(){ //整個可以移到computed
        //   // for(let i = 0; i<8; i++){ 傳統for判斷
        //   //     const line = lines[i];
        //   //     const [a, b, c] = line;
        //   //     const sum = this.grids[a] + this.grids[b] + this.grids[c];
        //   //     if(sum === 3) return 1;
        //   //     if(sum === -3) return -1;
        //   // }
        //   // return 0;
        //  //---------------------------------
        //     return lines.reduce((winner, [a, b, c])=>{ //高階用reduce
        //         const sum = this.grids[a] + this.grids[b] + this.grids[c];
        //         if(sum === 3) return 1;
        //         if(sum === -3) return -1;
        //         return 0;
        //     }, 0)
        // },
        reset(){
          this.grids = [0, 0, 0, 0, 0, 0, 0, 0, 0];
          this.player = 1;
          //this.winner = 0; //移到computed後就不需要再手動指定
        }
    }
}
</script>

<style>
.board{
  display: flex;
  flex-flow: row wrap;
  width: 400px;
  height: 400px;
  align-content: flex-start;
}

.grid{
  width: 33%;
  height: 33%;
  border: 1px solid gray;
  box-sizing: border-box;
  font-size: 60px;
  text-align: center;
  line-height: 130px;
}
.info{
  font-size: 40px;
}
</style>
