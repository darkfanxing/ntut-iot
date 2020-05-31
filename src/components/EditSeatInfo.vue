<template>
  <v-dialog v-model="dialog" max-width="290">
    <v-card>
      <v-card-title class="headline">Change Seat Info.</v-card-title>

      <v-card-text class="pb-0">
        <v-text-field
          outlined
          label="Seat ID"
          v-model="seat.id"
          hide-details="auto"
          :error="error"
          :rules="[checkId || '找不到座位 ID']"
          @keyup.enter="updateSeatInfo"
        ></v-text-field>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" text @click="closeEditSeatInfoDialog">
          取消
        </v-btn>
        <v-btn color="green darken-1" text @click="updateSeatInfo">儲存</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { rtdb } from "@/plugins/db";
import EventBus from "@/plugins/event-bus";

export default {
  data() {
    return {
      chairTypes: ["type-one", "type-two"],
      error: false
    };
  },
  props: {
    seat: Object,
    dialog: Boolean,
    seatsInfo: Object
  },
  computed: {
    checkId() {
      return this.seatsInfo[this.seat.id] != undefined;
    }
  },
  mounted() {
    if (this.dialog) {
      document.addEventListener(
        "keydown",
        e => {
          if (e.keyCode == 27) {
            this.closeEditSeatInfoDialog();
          }
        },
        {
          once: true
        }
      );
    }
  },
  methods: {
    closeEditSeatInfoDialog() {
      EventBus.$emit("closeEditSeatInfoDialog");
    },
    updateSeatInfo() {
      let rowIndex = this.seat.location[0];
      let columnIndex = this.seat.location[1];

      if (this.checkId) {
        rtdb.ref(`seats/${rowIndex}/${columnIndex}`).update({
          id: this.seat.id
        });

        this.error = false;
        this.closeEditSeatInfoDialog();
      } else {
        this.error = true;
      }
    }
  }
};
</script>
