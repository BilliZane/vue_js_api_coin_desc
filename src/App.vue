<template>
  <main class="main" v-cloak>
    <h1>Bitcoin Price Index</h1>

    <div v-for="currency in info" :key="currency.code">
      {{ `${currency.description}:` }}
      <strong>
        <span v-html="currency.symbol"></span>
        {{ roundVal(currency.rate_float) }}
      </strong>
    </div>

    <div v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this information at the moment,
        please try back later.
      </p>
    </div>
  </main>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      info: null,
      errored: false
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then((response) => (this.info = response.data.bpi))
      .catch((error) => {
        console.log(error)
        this.errored = true
      })
  },
  methods: {
    roundVal (value) {
      return value.toFixed(2)
    }
  }
}
</script>

<style lang="scss">
[v-cloak] {
    display: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.main {
  margin: 0 auto;
  width: 300px;
  padding: 0 10px 20px 10px;
  border: solid 2px #2c3e50;
  border-radius: 7px;
}

.main h1 {
  border-bottom: dotted 2px #2c3e50;
}
</style>
