<template>
  <v-app id="app" class="min-width-420">
    <v-app-bar app color="primary" class="min-width-420" dark>
      <v-icon size="32">mdi-alpha-i</v-icon>
      <v-icon size="32">mdi-alpha-o</v-icon>
      <v-icon size="32">mdi-alpha-t</v-icon>
      <h2>座位即時回報系統</h2>
      <v-spacer></v-spacer>
      <div class="hidden-sm-and-down">
        <v-icon
          v-for="title in titles"
          :key="title.icon"
          size="36"
          class="mr-5"
          :color="title.color"
          @click="changePage(title.name)"
        >
          {{ title.icon }}
        </v-icon>
      </div>
      <v-app-bar-nav-icon
        class="hidden-md-and-up"
        @click.stop="drawer = !drawer"
      ></v-app-bar-nav-icon>
    </v-app-bar>

    <v-content>
      <component :is="componentId"></component>
    </v-content>

    <v-navigation-drawer v-model="drawer" absolute right temporary>
      <v-list nav dense>
        <v-list-item-group
          v-model="selectTitle"
          active-class="deep-purple--text text--accent-4"
        >
          <v-list-item
            v-for="title in titles"
            :key="title.name"
            class="my-3"
            @click="changePage(title.name)"
          >
            <v-icon size="36" class="mr-5" :color="title.color">
              {{ title.icon }}
            </v-icon>
            <span class="title">{{ title.name }}</span>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
import SeatTable from "@/components/SeatTable";
import Analysis from "@/components/Analysis/Analysis";

export default {
  components: {
    SeatTable,
    Analysis
  },
  watch: {
    selectTitle() {
      this.drawer = false;
    }
  },
  data() {
    return {
      componentId: "SeatTable",
      drawer: false,
      selectTitle: null,
      titles: [
        {
          icon: "mdi-home",
          name: "Home",
          color: "#00e5ff"
        },
        {
          icon: "mdi-chart-bar",
          name: "Analysis",
          color: "#ffab00"
        }
      ]
    };
  },
  methods: {
    changePage(page) {
      if (page == "Home") {
        this.componentId = "SeatTable";
      } else {
        this.componentId = "Analysis";
      }
    }
  }
};
</script>

<style>
* {
  font-family: "微軟正黑體" !important;
}

.min-width-420 {
  min-width: 420px;
}
</style>
