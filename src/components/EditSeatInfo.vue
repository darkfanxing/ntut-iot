<template>
  <v-dialog v-model="dialog" persistent max-width="290">
    <v-card>
      <v-card-title class="headline">Change Seat Info.</v-card-title>

      <v-card-text class="pb-0">
        <v-text-field
          outlined
          label="Seat ID"
          v-model="seat.id"
          hide-details="auto"
        ></v-text-field>

        <v-select
          v-model="seat.chairType"
          class="mt-5"
          :items="chairTypes"
          label="Chair Type"
          outlined
        ></v-select>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" text @click="dialog = false">取消</v-btn>
        <v-btn color="green darken-1" text @click="updateSeatInfo">儲存</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import EventBus from "@/plugins/event-bus";

export default {
  data() {
    return {
      chairTypes: ["type-one", "type-two"]
    };
  },
  props: {
    seat: Object,
    dialog: Boolean
  },
  methods: {
    updateSeatInfo() {
      EventBus.$emit("updateSeatInfo", this.seat);
    }
  }
};
</script>
