<template>
  <div>
    <v-row dense>
      <v-col cols="12" md="6">
        <DataGauge />
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

export default {
  components: {
    SeatUsuageChart,
    DataGauge
  }
};
</script>
