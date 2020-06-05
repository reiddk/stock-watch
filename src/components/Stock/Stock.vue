<template>
  <div class="Stock">
    <div class="box-shadow stock-info">
      <div class="graphic" v-if="open">
        <Graphic :negative="negative" :high="high" :low="low" :currPrice="currPrice"/>
      </div>
      <div class="text-info">
          <div style="padding-left:15px;">
                    <h4>{{stockNameSymbol.name}}</h4>
        <div class="symbol-name">{{stockNameSymbol.symbol}}</div>
        <div class="main-stock">
          <div class="curr-price" v-if="stockInfo">{{currPrice.toFixed(2)}}</div>
          <div class="diff-price" v-bind:class="{'green':!negative, 'red': negative}">
            <Arrow v-bind:negative="negative" />&nbsp;{{diff.toFixed(2)}}&nbsp;({{diffPercent.toFixed(2)}}%)
          </div>
        </div>
        <div class="secondary-stock symbol-name">
          <span>OPEN {{open}}</span>
          <span>HIGH {{high}}</span>
          <span>LOW {{low}}</span>
        </div>
        </div>
          </div>
        </div>

  </div>
</template>

<script>
import Arrow from './Arrow.vue'
import Graphic from './Graphic.vue'

export default {
  name: 'Stock',
  components: {
    Arrow,
    Graphic
  },
  props: ['stockNameSymbol'],
  data () {
    return {
      errorMessage: '',
      stockInfo: null,
      currPrice: 0,
      diff: 0,
      diffPercent: 0,
      negative: false,
      open: 0,
      high: 0,
      low: 0
    }
  },
  mounted () {
    this.getStockInfo()
  },
  methods: {
    parseStockInfo () {
      if (this.stockInfo['05. price'] && this.stockInfo['02. open']) {
        this.high = Number(this.stockInfo['03. high'])
        this.low = Number(this.stockInfo['04. low'])
        this.currPrice = Number(this.stockInfo['05. price'])
        this.open = Number(this.stockInfo['02. open'])
        this.diff = Math.abs(this.currPrice - this.open)
        this.diffPercent = Math.abs(((this.currPrice - this.open) / this.open) * 100)
        if ((this.currPrice - this.open) < 0) {
          this.negative = true
        } else {
          this.negative = false
        }
      }
      console.log(this.stockInfo, this.stockNameSymbol)
    },
    async getStockInfo () {
      const out = await fetch(`https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=${this.stockNameSymbol.symbol}&apikey=HY0JP87WH3PG17X6`)
        .then(out => out.json())
        .then(out => out['Global Quote'] ? out['Global Quote'] : null)
        .catch(e => {
          console.log(e)
          return null
        })
      if (out === null) {
        this.errorMessage = 'Something went wrong getting the stock information'
      }
      this.stockInfo = out
      this.parseStockInfo()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

</style>
