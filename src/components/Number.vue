<!-- 
  实现目标如下：
  1. amount和firstId的双向数据绑定
  2. amount和firstId的监视，响应式更新pokemons数据
-->

<template>
  <div id="number">
    <div class="num-container">
      <label for="amount">Amount: </label>
      <input type="number" id="amount" min="1" max="100" v-model.number.lazy="amount" @change="amount = Math.trunc(amount)">
    </div>
    <div class="num-container">
      <label for="firstId">First id: </label>
      <input type="number" id="firstId" min="1" v-model.number.lazy="firstId" @change="firstId = Math.trunc(firstId)">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Number',
  data() {
    return {
      amount: Number(localStorage.getItem('amount') || 5), 
      firstId: Number(localStorage.getItem('firstId') || 1),
    }
  },
  methods: {
    //封装fetch请求
    getPokemon(i) {
      fetch(`https://pokeapi.co/api/v2/pokemon/${i}`)
          .then(response => response.json())
          .then(data => this.$bus.$emit('pushPokemon', data))
    },
    //pokemons数据为空时，初始化pokemons
    initPokemons() {
      for (let i = this.firstId; i < this.firstId + this.amount; i++) {
        this.getPokemon(i)
      }
    },
  },
  created() {
    //传给List组件
    this.$bus.$on('initPokemons', this.initPokemons)
  },
  watch: {
    amount(val, old) {
      //限制amount取值范围
      if (val < 1)   this.amount = 0
      if (val > 100) this.amount = 100

      if (val + this.firstId -1 > 898) {alert('超出宝可梦总数898！'); this.amount = 0}
      else {
        //比较新值与旧值，执行push或pop  
        if (val > old) {
          //for循环逐一获取pokemon对象
          for (let i = old + this.firstId ; i < val + this.firstId; i++) {
            this.getPokemon(i)
          }
        } else {
          //for循环逐一删除pokemon对象
          for (let i = old; i > val; i--) {
            this.$bus.$emit('popPokemon')
          }
        }
      }
      localStorage.setItem('amount', val)
    },
    firstId(val, old) {
      //设定firstId最小值
      if (val < 1)  {this.firstId = 1}

      if (val + this.amount -1 > 898) {alert('超出宝可梦总数898！'); this.firstId = old}
      else {
        this.$bus.$emit('clearPokemons') //清空pokemons数据
        for (let i = val; i < val + this.amount; i++) {
        this.getPokemon(i)
        }
      }
      localStorage.setItem('firstId', val)
    }
  },
}
</script>

<style scoped>
  #number {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .num-container {
    display: inline-block;
    width: 161px;
    padding: 0 10px 0 30px;
    height: 50px;
    line-height: 50px;
    background-color: white;
    border-radius: 25px;
    overflow: hidden;
    margin: 10px;
    text-align: center;
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.1);
  }
  input {
    width: 50px;
    height: 25px;
    border: 0;
    font-size: 17px;
    color: #0074ec;
    border-radius: 8px;
    text-align: center;
    outline: 0;
  }
</style>