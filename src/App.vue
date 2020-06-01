<template>
  <v-app id="app" class="min-width-420">
    <v-app-bar color="primary" class="min-width-420" dark app>
      <v-icon size="32">mdi-alpha-i</v-icon>
      <v-icon size="32">mdi-alpha-o</v-icon>
      <v-icon size="32">mdi-alpha-t</v-icon>
      <h3>座位即時回報系統</h3>
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
    <v-navigation-drawer
      v-model="drawer"
      absolute
      right
      temporary
      :width="navigationWidth"
      style="z-index: 999999;"
    >
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
            <v-icon
              size="36"
              class="mr-5"
              :color="title.color == 'white' ? 'black' : title.color"
            >
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
    },
    drawer() {
      if (this.drawer) {
        this.navigationWidth = 260;
      } else {
        this.navigationWidth = 0;
      }
    }
  },
  data() {
    return {
      componentId: "SeatTable",
      drawer: false,
      navigationWidth: 0,
      selectTitle: null,
      titles: [
        {
          icon: "mdi-home",
          name: "Home",
          color: "white"
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
  min-width: 300px;
}
</style>
