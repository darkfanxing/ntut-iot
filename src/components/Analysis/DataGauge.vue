<template>
  <div id="chart" class="d-flex justify-center mt-10">
    <VueApexChart
      type="radialBar"
      height="350"
      style="max-width: 100vw;"
      :options="chartOptions"
      :series="series"
    ></VueApexChart>
  </div>
</template>

<script>
import VueApexChart from "vue-apexcharts";

export default {
  props: {
    series: Array,
    label: String
  },
  data() {
    return {
      chartOptions: {
        chart: {
          height: 350,
          width: 300,
          type: "radialBar",
          offsetY: -10
        },
        plotOptions: {
          radialBar: {
            startAngle: -135,
            endAngle: 135,
            dataLabels: {
              name: {
                fontSize: "16px",
                color: undefined,
                offsetY: 120
              },
              value: {
                offsetY: 76,
                fontSize: "22px",
                color: undefined,
                formatter: function(val) {
                  return val + "%";
                }
              }
            }
          }
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "dark",
            shadeIntensity: 0.15,
            inverseColors: false,
            opacityFrom: 1,
            opacityTo: 1,
            stops: [0, 50, 65, 91]
          }
        },
        stroke: {
          dashArray: 4
        },
        labels: [this.label]
      }
    };
  },
  components: {
    VueApexChart
  }
};
</script>
