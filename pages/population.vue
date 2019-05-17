<template>
    <section class="container">
    <div>
      <h1 class="title">The 10 most populous countries</h1>
      <!--no-ssr : useless as this is a SPA -->
        <bar-chart 
          id="bar" :data="countries" xkey="countryiso3code" ykeys='["value"]' labels='["Pop."]' resize="true"
          bar-colors='[ "#36A2EB" ]'
          grid="true" grid-text-weight="bold">
        </bar-chart>
      <!-- /no-ssr -->
    </div>
  </section>
</template>

<script>
import Vue from 'vue'
import { BarChart } from 'vue-morris'

export default {
  components: {
    BarChart
  },

  data() {
    return {
      countries: []
    }
  },

  mounted() {
    this.$axios.$get('http://api.worldbank.org/v2/country/all/indicator/SP.POP.TOTL?date=2018&format=json&per_page=300&mrv=1')
      .then(data => {
        this.countries = this._.chain(data[1]).filter(d => d.value && d.countryiso3code).sortBy('value').value().slice(-10).reverse()
      })
  }
}
</script>

<style>
.container {
  text-align: center;
  max-width: 80%;
  margin: auto;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>
