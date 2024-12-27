<template>
  <div @click="showGraph = !showGraph" class="row">
    <div class="cell">{{ indicator.indicator }}</div>
    <div class="cell this-day">{{ indicator.this_day }}</div>
    <div class="cell procent"
      :class="{ 'positive-cell': percentageDifference > 0, 'negative-cell': percentageDifference < 0 }">
      <div>{{ indicator.yesterday }}</div>
      <div :class="{ 'positive': percentageDifference >= 0, 'negative': percentageDifference < 0 }">{{
        percentageDifference }}%</div>
    </div>
    <div class="cell" :class="{ 'positive-this-week': percentageDifference > 0, 'negative-this-week': percentageDifference < 0 }">
      {{ indicator.this_day_week }}</div>
  </div>
  <canvas v-if="showGraph" id="myChart" ref="myChart"></canvas>
</template>

<script>
import { Chart, registerables } from 'chart.js'; // Импортируем Chart.js
import { nextTick } from 'vue'; // Импортируем nextTick

Chart.register(...registerables); // Регистрируем компоненты Chart.js

export default {
  props: {
    indicator: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      percentageDifference: null,
      chart: null,
      showGraph: false,
    };
  },
  mounted() {
    this.calculatePercentageDifference();
    this.renderChart(); // Вызов метода для отрисовки графика
  },
  methods: {
    calculatePercentageDifference() {
      const thisDay = this.indicator.this_day;
      const yesterday = this.indicator.yesterday;

      if (thisDay !== null && yesterday !== null) {
        const diff = thisDay - yesterday;
        const average = (thisDay + yesterday) / 2;
        this.percentageDifference = ((diff / average) * 100).toFixed(0);
      } else {
        this.percentageDifference = null;
      }
    },
    renderChart() {
      if (this.chart) {
        this.chart.destroy();
      }
      nextTick(() => {
        const ctx = this.$refs.myChart.getContext('2d');
        this.chart = new Chart(ctx, {
          type: 'line',
          data: {
            labels: ['6 дней назад', '5 дней назад', '4 дня назад', '3 дня назад', '2 дня назад', 'Вчера', 'Сегодня'],
            datasets: [{
              label: this.indicator.indicator,
              data: [
                this.indicator.this_day_week * (0.8 + Math.random() * 0.4),
                this.indicator.this_day_week * (0.75 + Math.random() * 0.5),
                this.indicator.this_day_week * (0.7 + Math.random() * 0.6),
                this.indicator.this_day_week * (0.65 + Math.random() * 0.7),
                this.indicator.this_day_week * (0.6 + Math.random() * 0.8),
                this.indicator.yesterday,
                this.indicator.this_day
              ],
              borderColor: '#42A5F5',
              fill: false,
              tension: 0.1
            }]
          },
          options: {
            responsive: true,
            scales: {
              y: {
                beginAtZero: true
              }
            }
          }
        });
      });
    }
  },
  watch: {
    showGraph(newValue) {
      if (newValue) {
        this.renderChart(); // Перерисовываем график при открытии
      } else if (this.chart) {
        this.chart.destroy(); // Уничтожаем график при закрытии
      }
    }
  }
};
</script>

<style scoped>
.row {
  display: flex;
  width: 100%;
}

.cell {
  width: 160px;
  text-align: right;
  padding: 15px;
  background-color: rgba(227, 227, 227, 0.886);
  margin: 5px;
}

.cell:first-child {
  width: 500px;
  text-align: left;
}

.procent {
  display: flex;
  justify-content: space-around;
}

.positive {
  color: green;
}

.positive-cell {
  display: flex;
  justify-content: space-around;
  width: 160px;
  background-color: rgb(175, 255, 175);
  padding: 15px;
  margin: 5px;
}

.positive-this-week {
  text-align: right;
  width: 160px;
  background-color: rgb(175, 255, 175);
  padding: 15px;
  margin: 5px;
}

.negative {
  color: red;
}

.negative-cell {
  display: flex;
  justify-content: space-around;
  width: 160px;
  padding: 15px;
  background-color: rgb(255, 166, 166);
  margin: 5px;
}
.negative-this-week {
  text-align: right;
  width: 160px;
  padding: 15px;
  background-color: rgb(255, 166, 166);
  margin: 5px;
}

.negative-cell {
  display: flex;
  justify-content: space-around;
  width: 160px;
  padding: 15px;
  background-color: rgb(255, 166, 166);
  margin: 5px;
}

.this-day {
  background-color: rgb(127, 238, 255);
}
</style>