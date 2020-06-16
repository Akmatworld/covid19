<script>
import { Bar } from 'vue-chartjs'

export default {
  extends: Bar,
  props: ['chartData', 'country'],
  mounted () {
    this.renderChart({
      labels: ['Infected', 'Recovered', 'Deaths'],
      datasets: [
        {
          label: 'People',
          backgroundColor: ['rgba(0, 0, 255, 0.5)', 'rgba(0, 255, 0, 0.5)', 'rgba(255, 0, 0, 0.5)'],
          data: this.chartData
        }
      ]
    },
    {
      legend: { display: false },
      title: { display: true, text: `Current state in ${this.country}` },
      responsive: true,
      maintainAspectRatio: false
    })
  }
}
</script>
