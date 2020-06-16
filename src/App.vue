<template>
  <div id="app">
    <div class="logo">
        <img src="./assets/icon.png" alt="Covid logo">
    </div>
    <!-- cards -->
    <div class='card-container'>
        <MainCard class='card-1' card-title='Infected' :card-number='confirmed' :card-date='lastUpdate' card-text='Number of active cases of COVID-19'></MainCard>
        <MainCard class='card-2' card-title='Recovered' :card-number='recovered' :card-date='lastUpdate' card-text='Number of recoveries from COVID-19'></MainCard>
        <MainCard class='card-3' card-title='Deaths' :card-number='deaths' :card-date='lastUpdate' card-text='Number of deaths caused by COVID-19'></MainCard>
    </div>
    <!-- countries -->
    <div class="countries-container">
        <span class="select-country-title">Select country</span>
        <select v-model='selectedCountry'>
        <option value="global">Global</option>
        <option v-for="country in countries" :value='country.name' :key='country.name'>{{ country.name }}</option>
        </select>
    </div>
    <!-- chart -->
    <div class="chart-container">
        <BarChart v-if='country' :country='country' :chart-data='chartData'></BarChart>
        <LineChart v-else-if='chartData' :chart-data='chartData'></LineChart>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import MainCard from './components/main-card.vue'
import LineChart from './components/line-chart.vue'
import BarChart from './components/bar-chart.vue'

export default {
  name: 'App',
  components: {
    MainCard,
    LineChart,
    BarChart
  },
  data: function () {
    return {
      url: 'https://covid19.mathdro.id/api/',
      confirmed: 0,
      recovered: 0,
      deaths: 0,
      lastUpdate: '',
      countries: '',
      chartData: '',
      selectedCountry: 'global',
      country: ''
    }
  },
  created () {
    this.getGlobalData(this.url)
    this.getCountries(this.url)
    this.getChartData(this.url)
  },
  methods: {
    getGlobalData (url, country) {
      if (country) url += `countries/${country}`
      axios.get(url)
        .then((response) => {
          this.confirmed = response.data.confirmed.value
          this.recovered = response.data.recovered.value
          this.deaths = response.data.deaths.value
          this.lastUpdate = new Date(response.data.lastUpdate).toDateString()
          if (country) {
            this.chartData = [
              this.confirmed,
              this.recovered,
              this.deaths
            ]
            this.country = this.selectedCountry
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    getCountries (url) {
      url += 'countries'
      axios.get(url).then((response) => {
        this.countries = response.data.countries
      }).catch(function (error) {
        console.log(error)
      })
    },
    getChartData (url) {
      axios.get(url + 'daily').then((response) => {
        this.chartData = response.data.map(({ confirmed, deaths, reportDate: date }) => ({ confirmed: confirmed.total, deaths: deaths.total, date }))
      }).catch(function (error) {
        console.log(error)
      })
    }
  },
  watch: {
    selectedCountry (val, oldVal) {
      this.country = ''
      this.chartData = ''
      if (this.selectedCountry === 'global') {
        this.getGlobalData(this.url)
        this.getCountries(this.url)
        this.getChartData(this.url)
      } else {
        this.getGlobalData(this.url, val)
      }
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.logo {
  display: flex;
  justify-content: center;
  margin: 20px auto;
}
.card-1 {
  border-bottom: 8px solid #a6aaf8;
}
.card-2 {
  border-bottom: 8px solid #8fe58a;
}
.card-3 {
  border-bottom: 8px solid #ff8b87;
}
.md-field {
  width: 240px !important;
}
.select-country-title {
  margin-right: 10px;
}
.countries-container {
    display: flex;
    justify-content: center;
    margin-top: 15px;
}
.chart-container {
  display: flex;
  justify-content: center;
  margin: 0 auto;
}
.chart-container div {
  width: 80%;
}
.chart-container canvas {
  width: 40%;
  height: 40%;
}
.card-container {
  display: flex;
  justify-content: center;
  flex-flow: row wrap;
}
/* on screens that are 993 or large */
@media screen and (max-width: 769px) {
  .card-container {
    display: flex;
    justify-content: center;
  }
  .logo img {
    width: 80%;
  }
}
</style>
