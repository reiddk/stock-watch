<template>
  <div class="Stocks">
    <div class="" v-for="stock in StocksArr" :key="stock.symbol">
      <Stock v-bind:stockNameSymbol="stock" />
    </div>
  </div>
</template>

<script>
import Stock from './Stock/Stock.vue'

export default {
  name: 'Stocks',
  components: {
    Stock
  },
  props: {
    StocksArr: Array
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.Stocks {
  display:flex;
  flex-wrap:wrap;
  flex-basis:100px;
}
</style>
