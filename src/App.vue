<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-icon size="32">mdi-alpha-i</v-icon>
      <v-icon size="32">mdi-alpha-o</v-icon>
      <v-icon size="32">mdi-alpha-t</v-icon>
      <h2>座位即時回報系統</h2>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <SeatTable />
    </v-content>
  </v-app>
</template>

<script>
import SeatTable from "@/components/SeatTable";

export default {
  components: {
    SeatTable
  }
};
</script>

<style>
* {
  font-family: "微軟正黑體" !important;
}
</style>
