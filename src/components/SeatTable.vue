<template>
  <div class="pt-5 d-flex flex-column justify-space-around fill-height">
    <ActionButton class="my-5" :seats="seats" />
    <div class="scroll" style="max-height: 70vh;">
      <table>
        <tr v-for="(rowSeats, rowIndex) in seats" :key="`row-${rowIndex}`">
          <draggable
            v-model="seats"
            draggable=".seat"
            group="a"
            :move="handleMove"
            @end="handleDragEnd"
          >
            <td
              v-for="(seat, columnIndex) in rowSeats"
              :key="`${rowIndex}-${columnIndex}`"
              class="seat pa-5"
              style="color: black;"
              :id="`${rowIndex}-${columnIndex}`"
              align="center"
              valign="middle"
              @dblclick="
                editSeatId({
                  id: seat.id,
                  location: [rowIndex, columnIndex],
                  isEmpty: seat.isEmpty,
                  chairType: seat.chairType
                })
              "
            >
              <div
                v-if="seat.chairType == 'type-two'"
                class="d-flex flex-column fill-height justify-space-around"
              >
                <div
                  v-for="index in 2"
                  :key="`type-tow-${index}`"
                  :class="[
                    seatsIsEmpty[seat.id][index - 1]
                      ? 'is-empty'
                      : 'is-not-empty',
                    index == 1 ? 'round-top' : 'round-bottom',
                    'd-flex flex-column align-center justify-center type-one'
                  ]"
                >
                  {{ index }}{{ seat.id }}{{ index == 1 ? "（左）" : "（右）" }}
                </div>
              </div>

              <div
                v-else-if="seat.chairType == 'type-one'"
                :class="[
                  seatsIsEmpty[seat.id][0] ? 'is-empty' : 'is-not-empty',
                  'd-flex justify-center align-center type-one round',
                  seat.id == 'None' ? 'no-id' : ''
                ]"
                style="height: 100%; width: 100%"
              >
                {{ seat.id != "None" ? seat.id : "無座位" }}
              </div>
            </td>
          </draggable>
        </tr>
      </table>

      <editSeatInfo :seat="editSeat" :dialog="dialog" />
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import editSeatInfo from "@/components/EditSeatInfo";
import ActionButton from "@/components/ActionButton";

import { frdb } from "@/plugins/db";
import { rtdb } from "@/plugins/db";
import EventBus from "@/plugins/event-bus";

export default {
  components: {
    draggable,
    editSeatInfo,
    ActionButton
  },
  data() {
    return {
      editSeat: {},
      dialog: false,
      dragElement: null,
      dropElement: null,
      seatsIsEmpty: [],
      seats: []
    };
  },
  mounted() {
    EventBus.$on("updateSeatInfo", newSeat => {
      let location = newSeat.location;

      delete newSeat.location;
      this.seats[location[0]][location[1]] = newSeat;
      this.dialog = false;
    });

    EventBus.$on("changeSeatTable", newSeat => {
      this.seats = newSeat;
    });
  },
  methods: {
    handleDragEnd() {
      let dragRowIndex = this.dragElement[0];
      let dragColumnIndex = this.dragElement[1];

      let dropRowIndex = this.dropElement[0];
      let dropColumnIndex = this.dropElement[1];

      let seats = Object.assign([], this.seats);
      let temp = seats[dragRowIndex][dragColumnIndex];

      seats[dragRowIndex][dragColumnIndex] =
        seats[dropRowIndex][dropColumnIndex];

      seats[dropRowIndex][dropColumnIndex] = temp;

      this.seats = seats;
    },
    handleMove({ dragged, related }) {
      let relatedElement = dragged.id;
      let draggedElement = related.id;

      if (relatedElement != undefined && draggedElement != undefined) {
        this.dropElement = relatedElement.split("-");
        this.dragElement = draggedElement.split("-");
      }
      return false;
    },
    editSeatId(seat) {
      this.editSeat = seat;
      this.dialog = true;
    }
  },
  firestore: {
    seatsIsEmpty: frdb.collection("seat-situation").doc("is-empty")
  },
  firebase: {
    seats: rtdb.ref("/seats")
  }
};
</script>

<style scoped>
@import "../css/scroll-bar.css";

.seat {
  /* border: black solid 2px; */
  width: 260px;
  height: 260px;
  min-height: 260px;
  min-width: 260px;
  color: white;
  font-size: 1.2em;
}

.is-empty {
  background-color: #4caf50;
}

.is-not-empty {
  background-color: #ff5252;
}

.type-one {
  height: 50% !important;
  width: 100% !important;
}

.no-id {
  background-color: #bdbdbd;
}

.round {
  border-radius: 15px;
}

.round-bottom {
  border-radius: 0 0 15px 15px;
  border-top: white 1px solid;
}

.round-top {
  border-radius: 15px 15px 0 0;
  border-bottom: white 1px solid;
}

.scroll {
  overflow-y: auto;
  overflow-x: auto;
}
</style>
