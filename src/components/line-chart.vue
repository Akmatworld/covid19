<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: ['chartData'],
  mounted () {
    this.renderChart({
      labels: this.chartData.map((data) => data.date),
      datasets: [
        {
          label: 'Infected',
          borderColor: '#3333ff',
          data: this.chartData.map((data) => data.confirmed),
          fill: true
        },
        {
          label: 'Deaths',
          borderColor: '#f87979',
          backgroundColor: 'rgba(255, 0, 0, 0.5)',
          data: this.chartData.map((data) => data.deaths),
          fill: true
        }
      ]
    }, { responsive: true, maintainAspectRatio: false })
  }
}
</script>
