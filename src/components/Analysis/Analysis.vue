<template>
  <div>
    <v-row dense>
      <v-col cols="12" md="6">
        <DataGauge
          :series="[dataInfo[Object.keys(dataInfo)[0]]['溫度']]"
          label="溫度"
        />
        <DataGauge
          :series="[dataInfo[Object.keys(dataInfo)[0]]['濕度']]"
          label="濕度"
        />
      </v-col>
      <v-col cols="12" md="6">
        <SeatUsuageChart />
      </v-col>
    </v-row>
  </div>
</template>

<script>
import SeatUsuageChart from "@/components/Analysis/SeatUsuageChart";
import DataGauge from "@/components/Analysis/DataGauge";

import { rtdb } from "@/plugins/db";

export default {
  data() {
    return {
      dataInfo: {}
    };
  },
  components: {
    SeatUsuageChart,
    DataGauge
  },
  firebase: {
    dataInfo: rtdb.ref("/seat-info").limitToLast(1)
  }
};
</script>
