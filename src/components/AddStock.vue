<template>
  <div class="AddStock">
      <input class="big-input" placeholder="Enter stock symbol" :value="stockToAdd.toUpperCase()" @input="stockToAdd = $event.target.value.toUpperCase()" v-on:keyup.enter="setStock()"/>
      <button class="box-shadow big-button" type="button" v-on:click="setStock()"><span></span></button>
      <div v-show="loading" class="loading-icon"></div>
      <div class="error-message" v-bind:class="{'show':errorMessage.length}">{{errorMessage}}</div>
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
      errorMessage: '',
      loading: false
    }
  },
  methods: {
    stockAlreadySet () {
      console.log(this.CurrStocks, this.stockToAdd)
      for (const s of this.CurrStocks) {
        if (s.symbol.toUpperCase() === this.stockToAdd.toUpperCase()) {
          return true
        }
      }
      return false
      // return this.CurrStocks.includes(s => { console.log(s); return s.symbol.toUpperCase() === this.stockToAdd.toUpperCase() })
    },
    async stockValid () {
      const stock = await fetch(process.env.VUE_APP_API_VALIDATE_STOCK.replace('API_KEY', process.env.VUE_APP_API_KEY).replace('THE_STOCK', this.stockToAdd))
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
      this.loading = true
      const stockNameSymbol = await this.stockValid()
      this.loading = false
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
