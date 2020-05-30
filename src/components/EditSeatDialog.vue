<template>
  <v-dialog v-model="dialog" persistent max-width="290">
    <v-card>
      <v-card-title class="headline">Change Seat ID</v-card-title>
      <v-card-text class="pb-0">
        <v-text-field
          solo
          :label="seat.id"
          v-model="seat.id"
          hide-details="auto"
        ></v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" text @click="dialog = false">取消</v-btn>
        <v-btn color="green darken-1" text @click="updateId">儲存</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import EventBus from "@/plugins/event-bus";

export default {
  props: {
    seat: Object,
    dialog: Boolean
  },
  methods: {
    updateId() {
      EventBus.$emit("updateId", this.seat);
    }
  }
};
</script>
