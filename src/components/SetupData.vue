<template>
  <div class="row">
    <!-- For loop on the charts in each row. -->
    <div v-for="(col, index) in columns" :key="index" :class="columns[index]">
      <!-- Add h1 tag with the title if existed -->
      <h1 v-if="chartData[index].title != ''">{{ chartData[index].title }}</h1>
      <h1 v-else class="title"></h1>

      <!-- Switch case is not stable when imported! Either from CDN or Npm-->
      <!-- Using if statements until error resolved. -->
      <BarChart
        v-if="chartData[index].type === 'Bar'"
        :chartdata="{
          labels: chartData[index].labels,
          datasets: chartData[index].datasets,
        }"
        :style="{
          height: chartData[index].height + 'px',
          width: chartData[index].width + 'px',
        }"
        :options="options"
        class="chart"
      />

      <LineChart
        v-if="chartData[index].type === 'Line'"
        :chartdata="{
          labels: chartData[index].labels,
          datasets: chartData[index].datasets,
        }"
        :style="{
          height: chartData[index].height + 'px',
          width: chartData[index].width + 'px',
        }"
        :options="options"
        class="chart"
      />

      <PieChart
        v-if="chartData[index].type === 'Pie'"
        :chartdata="{
          labels: chartData[index].labels,
          datasets: chartData[index].datasets,
        }"
        :style="{
          height: chartData[index].height + 'px',
          width: chartData[index].width + 'px',
        }"
        :options="options"
        class="chart"
      />

      <RadarChart
        v-if="chartData[index].type === 'Radar'"
        :chartdata="{
          labels: chartData[index].labels,
          datasets: chartData[index].datasets,
        }"
        :style="{
          height: chartData[index].height + 'px',
          width: chartData[index].width + 'px',
        }"
        :options="options"
        class="chart"
      />

      <PolarArea
        v-if="chartData[index].type === 'Polar'"
        :chartdata="{
          labels: chartData[index].labels,
          datasets: chartData[index].datasets,
        }"
        :style="{
          height: chartData[index].height + 'px',
          width: chartData[index].width + 'px',
        }"
        :options="options"
        class="chart"
      />

      <BubbleChart
        v-if="chartData[index].type === 'Bubble'"
        :chartdata="{
          labels: chartData[index].labels,
          datasets: chartData[index].datasets,
        }"
        :style="{
          height: chartData[index].height + 'px',
          width: chartData[index].width + 'px',
        }"
        :options="options"
        class="chart"
      />
    </div>
  </div>
</template>

<script>
// Import charts from their components.
import BarChart from "./BarChart.vue";
import LineChart from "./LineChart.vue";
import PieChart from "./PieChart.vue";
import RadarChart from "./RadarChart.vue";
import PolarArea from "./PolarArea.vue";
import BubbleChart from "./BubbleChart.vue";

import Vue from "vue";
import axios from "axios";
import vueAxios from "vue-axios";
Vue.use(vueAxios, axios);

export default {
  name: "SetupData",
  props: ["data"],

  // Define components.
  components: {
    BarChart,
    LineChart,
    PieChart,
    RadarChart,
    PolarArea,
    BubbleChart,
  },

  // Define data for columns and chartData.
  data() {
    return {
      columns: [],
      chartData: [],
      options: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },

  // called on mounted.
  mounted() {
    this.SetupData();
  },

  methods: {
    // Responsible for setting up data.
    SetupData: async function () {
      // Sort the json file based on priority.
      this.data.charts.sort(function (a, b) {
        if (a.priority < b.priority) {
          return -1;
        }

        if (a.priority > b.priority) {
          return 1;
        }

        return 0;
      });

      // Loop through each row to prepare for setup
      for (let i = 0; i < this.data.charts.length; i++) {
        if (this.data.charts[i].api != "") {
          await this.GetData(this.data.charts[i].api, i);
        }
        // save each chart data in chartData with their respective index.
        this.chartData.push(this.data.charts[i]);
        // save each col value for each chart in their respective index.
        this.columns.push("col-" + this.data.charts[i].columns);
      }
    },
    GetData: async function (url, index) {
      // await for values to come from the url.
      await Vue.axios.get(url).then((resp) => {
        // if response is successfull.
        if (resp.status == 200) {
          // console.log(resp.data.datasets[0]);
          this.data.charts[index].labels = resp.data.labels;
          this.data.charts[index].datasets = resp.data.datasets;
        }
      });
    },
  },
};
</script>


<style >
.chart {
  border: 2px solid #000;
  box-shadow: 5px 10px 8px 10px #888888;
  background-color: rgb(255, 255, 255);
  margin: 10px;
}

.row {
  padding: 10px;
}

.title {
  padding-top: 49px;
}
</style>