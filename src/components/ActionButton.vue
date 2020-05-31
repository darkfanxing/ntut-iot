<template>
  <div>
    <div>
      <v-row>
        <v-col
          cols="12"
          md="3"
          class="text-center"
          v-for="(action, index) in actions"
          :key="`action-${index}`"
        >
          <v-btn @click="check(action)" rounded color="warning" dark>
            {{ action }}
          </v-btn>
        </v-col>
      </v-row>
    </div>

    <CheckAction :action="selectAction" :dialog="dialog" />
  </div>
</template>

<script>
import CheckAction from "@/components/CheckAction";

// import { frdb } from "@/plugins/db";
import { rtdb } from "@/plugins/db";
import EventBus from "@/plugins/event-bus";

export default {
  data() {
    return {
      actions: ["Add Row", "Remove Row", "Add Column", "Remove Column"],
      selectAction: "",
      dialog: false
    };
  },
  props: {
    seats: Array
  },
  mounted() {
    EventBus.$on("confirmAction", selectAction => {
      if (selectAction == "Add Row") {
        this.addRow();
      } else if (selectAction == "Remove Row") {
        this.removeRow();
      } else if (selectAction == "Add Column") {
        this.addColumn();
      } else if (selectAction == "Remove Column") {
        this.removeColumn();
      }

      this.dialog = false;
    });

    EventBus.$on("closeDialog", () => {
      this.dialog = false;
    });
  },
  components: {
    CheckAction
  },
  methods: {
    check(selectAction) {
      this.selectAction = selectAction;
      this.dialog = true;
    },
    addRow() {
      let rowSeats = [];
      for (
        let columnIndex = 0;
        columnIndex < this.seats[0].length;
        columnIndex++
      ) {
        rowSeats.push({
          id: "None",
          chairType: "type-one"
        });
      }
      rtdb.ref("/seats/" + this.seats.length).set(rowSeats);
    },
    removeRow() {
      let seatLength = this.seats.length - 1;
      if (this.seats.length - 1 >= 1) {
        rtdb.ref("/seats/" + seatLength).remove();
      }
    },
    addColumn() {
      let seats = this.seats;
      for (let rowIndex = 0; rowIndex < this.seats.length; rowIndex++) {
        seats[rowIndex].push({
          id: "None",
          chairType: "type-one"
        });
      }

      rtdb.ref("/seats").set(seats);
    },
    removeColumn() {
      if (this.seats[0].length - 1 >= 1) {
        let seats = this.seats;
        for (let rowIndex = 0; rowIndex < this.seats.length; rowIndex++) {
          seats[rowIndex].pop();
        }

        rtdb.ref("/seats").set(seats);
      }
    }
  }
};
</script>
