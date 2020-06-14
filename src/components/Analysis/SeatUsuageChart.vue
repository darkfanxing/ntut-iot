<template>
  <div id="chart" class="d-flex justify-center mt-10">
    <VueApexChart
      type="area"
      height="350"
      :options="chartOptions"
      :series="series"
      :key="test"
    ></VueApexChart>
  </div>
</template>

<script>
import VueApexChart from "vue-apexcharts";
import { rtdb } from "@/plugins/db";

export default {
  data() {
    return {
      dataInfo: {},
      test: 0,
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
  watch: {
    dataInfo() {
      this.updateInfo();
    }
  },
  methods: {
    updateInfo() {
      let series = this.series;

      let keys = Object.keys(this.dataInfo);
      keys.forEach(key => {
        series[0].data.push([
          new Date(
            this.dataInfo[key]["日期"] + " " + this.dataInfo[key]["時間"]
          ),
          this.dataInfo[key]["座位狀況"] == "空位" ? ["0"] : ["100"]
        ]);
      });

      this.series = series;
      this.test += 1;
    }
  },
  components: {
    VueApexChart
  },
  firebase: {
    dataInfo: rtdb.ref("/seat-info")
  }
};
</script>
