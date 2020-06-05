<template>
  <div id="app">
    <h1>Stock Watcher</h1>
    <addStock @add-stock="newStockAdded" v-bind:CurrStocks="StocksArr" />
    <Stocks v-bind:StocksArr="StocksArr" />
  </div>
</template>

<script>
import Stocks from './components/Stocks.vue'
import AddStock from './components/AddStock.vue'

export default {
  name: 'App',
  components: {
    Stocks,
    AddStock
  },
  data () {
    return {
      StocksArr: [
        {
          symbol: 'GOOG',
          name: 'ALPHABET INC. CL C'
        }
      ]
    }
  },
  methods: {
    newStockAdded (stockName) {
      this.StocksArr.push(stockName)
    }
  }
}
</script>

<style lang="scss">
body {
  background-color:$background;
}

</style>
