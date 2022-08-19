<!-- 
  实现目标如下：
  1. style绘制拟物风格卡片，flex响应式布局
  2. template动态绑定头像、id、name、type、卡片颜色
  3. script实现pokemons数据对象的增删操作
-->

<template>
  <div id="List">
    <div class="border" v-for="pokemon in pokemons" :key="pokemon.id">
      <div class="item" :style="{backgroundColor: colors[pokemon.types[0].type.name]}">
        <div class="img-container">
          <img :src="pokemon.sprites.front_default">
        </div>
        <div class="id">#{{pokemon.id.toString().padStart(3, '0')}}</div>
        <h3>{{pokemon.name}}</h3>
        <small>type: {{pokemon.types[0].type.name}}</small>
      </div>
    </div>
    <!-- <div class="border">
      <div class="item">
        <div class="img-container">
          <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/25.png">
        </div>
        <div class="id">#001</div>
        <h3>比卡超</h3>
        <small>type: electric</small>
      </div>
    </div> -->
  </div>
</template>

<script>
export default {
  name: 'List',
  data() {
    return {
      pokemons: [],
      colors: {
        fire: '#ff927d',
        grass: '#bdffa3',
        electric: '#ffcc33',
        water: '#77bbff',
        ground: '#f1dda0',
        rock: '#e1d08c',
        fairy: '#fbcbfb',
        poison: '#c689ba',
        bug: '#daec44',
        dragon: '#a194ff',
        psychic: '#ff9cc4',
        flying: '#99bbff',
        fighting: '#dd9988',
        normal: '#e7e7d8',
        ghost: '#9f9fec',
        steel: '#dfdfe1',
        ice: '#dbf6ff',
        dark: '#bda396',
      }
    }
  },
  methods: {
    pushPokemon(data) {
      this.pokemons.push(data)
    },
    popPokemon() {
      this.pokemons.pop()
    },
    clearPokemons() {
      this.pokemons = []
    },
    // pokemons数据为空时，初始化pokemons
    init() {
      //检测pokemons是否为空数组，涉及隐式类型转换
      if (!this.pokemons.toString()) this.$bus.$emit('initPokemons') 
    },
  },
  created() {
    // 传给Number组件
    this.$bus.$on('pushPokemon', this.pushPokemon)
    this.$bus.$on('popPokemon', this.popPokemon)
    this.$bus.$on('clearPokemons', this.clearPokemons)
    this.init() // 初始化pokemons
  },
}
</script>

<style scoped>
  #List {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  }
  .border {
    margin: 15px;
    background: white;
    width: 196px;
    height: 286px;
    border-radius: 10px;
    padding: 8px;
    box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.2),
                -2px -2px 5px rgba(0, 0, 0, 0.2) inset;
  }
  .item {
    background-color: #ffcc33;
    width: 180px;
    height: 270px;
    border-radius: 10px;
    padding: 15px;
    text-align: center;
  }
  .img-container {
    background-color: rgba(255, 255, 255, 0.6);
    border-radius: 50%;
    width: 120px;
    height: 120px;
    margin: auto;
  }
  img {
    width: 100%;
    margin-top: 0px;
    margin-left: 0px;
  }
  .id {
    font-size: 12px;
    margin: 15px auto;
    padding: 5px 10px;
    border-radius: 10px;
    width: fit-content;
    background: rgba(0, 0, 0, 0.1);
  }
  h3 {
    margin: 10px;
  }
  
</style>
