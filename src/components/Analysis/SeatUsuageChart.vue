<template>
  <div id="chart" class="d-flex justify-center mt-10">
    <VueApexChart
      type="area"
      height="350"
      width="500"
      :options="chartOptions"
      :series="series"
    ></VueApexChart>
  </div>
</template>

<script>
import VueApexChart from "vue-apexcharts";

export default {
  data() {
    return {
      series: [
        {
          data: []
        }
      ],
      chartOptions: {
        chart: {
          type: "area",
          stacked: false,
          height: 350,
          zoom: {
            type: "x",
            enabled: true,
            autoScaleYaxis: true
          },
          toolbar: {
            autoSelected: "zoom"
          }
        },
        dataLabels: {
          formatter: function(val) {
            return val + "%";
          }
        },
        fill: {
          type: "gradient",
          gradient: {
            shadeIntensity: 1,
            inverseColors: false,
            opacityFrom: 0.5,
            opacityTo: 0,
            stops: [0, 90, 100]
          }
        },
        yaxis: {
          title: {
            text: "座位使用率（%）",
            style: {
              fontSize: "18px"
            }
          }
        },
        xaxis: {
          type: "datetime"
        },
        tooltip: {
          shared: false,
          y: {
            formatter: function(val) {
              return val + "%";
            }
          }
        }
      }
    };
  },
  created() {
    for (let count = 0; count < 5; count++) {
      this.updateChart(count);
    }
  },
  methods: {
    updateChart(count) {
      let series = this.series;
      series[0].data.push([
        new Date().getTime() + 10000000 * count,
        Math.round(Math.random() * (100 - 0))
      ]);
      this.series = series;
    }
  },
  components: {
    VueApexChart
  }
};
</script>
