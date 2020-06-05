<template>
  <div class="AddStock">
      <input class="big-input" placeholder="Enter stock symbol" :value="stockToAdd.toUpperCase()" @input="stockToAdd = $event.target.value.toUpperCase()" v-on:keyup.enter="setStock()"/>
      <button class="box-shadow big-button" type="button" v-on:click="setStock()">ADD STOCK</button>
      <div class="error-message" v-show="errorMessage.length">{{errorMessage}}</div>
  </div>
</template>

<script>

export default {
  name: 'Stocks',
  props: {
    StocksArr: Array,
    CurrStocks: Array
  },
  data () {
    return {
      stockToAdd: '',
      errorMessage: ''
    }
  },
  methods: {
    stockAlreadySet () {
      return this.CurrStocks.includes(s => s.symbol === this.stockToAdd)
    },
    async stockValid () {
      const stock = await fetch(`https://www.alphavantage.co/query?function=SYMBOL_SEARCH&keywords=${this.stockToAdd}&apikey=HY0JP87WH3PG17X6`)
        .then(out => out.json())
        .then(out => {
          if (!out ||
            !out.bestMatches ||
            out.bestMatches.length === 0) {
            return null
          } else {
            const info = out.bestMatches.find(s => s['1. symbol'] === this.stockToAdd)
            if (info) {
              return {
                symbol: info['1. symbol'],
                name: info['2. name']
              }
            } else {
              return null
            }
          }
        })
        .catch(e => {
          console.log(e)
          return null
        })
      return stock
    },
    async setStock () {
      if (this.stockToAdd.length === 0) {
        this.errorMessage = 'Field is Empty'
        return
      } else if (this.stockAlreadySet()) {
        this.errorMessage = 'Stock has already been added'
        return
      }
      const stockNameSymbol = await this.stockValid()
      if (!stockNameSymbol) {
        this.errorMessage = 'Stock name is invalid'
        return
      }
      this.errorMessage = ''
      this.$emit('add-stock', stockNameSymbol)
      this.stockToAdd = ''
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
