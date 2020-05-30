<template>
  <div>
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
            :id="`${rowIndex}-${columnIndex}`"
            align="center"
            valign="middle"
            @dblclick="
              editSeatId({
                id: seat.id,
                location: [rowIndex, columnIndex],
                isEmpty: seat.isEmpty
              })
            "
          >
            <div
              v-if="seat.isTwoChair"
              class="d-flex flex-column fill-height justify-space-around two-chair"
            >
              <div
                style="height:50%; width: 100%"
                class="d-flex align-center justify-center"
                :class="[seat.isEmpty[0] ? 'is-empty' : 'is-not-empty']"
              >
                {{ seat.id + "（左）" }}
              </div>
              <hr />
              <div
                style="height:50%; width: 100%"
                class="d-flex align-center justify-center"
                :class="[seat.isEmpty[1] ? 'is-empty' : 'is-not-empty']"
              >
                {{ seat.id + "（右）" }}
              </div>
            </div>

            <div
              v-if="!seat.isTwoChair"
              :class="[
                seat.isEmpty[0] ? 'is-empty' : 'is-not-empty',
                'd-flex justify-center align-center one-chair'
              ]"
            >
              {{ seat.id }}
            </div>
          </td>
        </draggable>
      </tr>
    </table>

    <span @click="addRow">add row</span>
    <span @click="removeRow">remove row</span>
    <span @click="addColumn">add column</span>
    <span @click="removeColumn">remove column</span>

    <editSeatDialog :seat="editSeat" :dialog="dialog" />
  </div>
</template>

<script>
import draggable from "vuedraggable";
import editSeatDialog from "@/components/EditSeatDialog";

import EventBus from "@/plugins/event-bus";

export default {
  components: {
    draggable,
    editSeatDialog
  },
  data() {
    return {
      editSeat: {},
      dialog: false,
      rowCount: 2,
      columnCount: 2,
      dragElement: null,
      dropElement: null,
      seats: [
        [
          {
            id: "a0001",
            isEmpty: [true, false],
            isTwoChair: true
          },
          {
            id: "a0001",
            isEmpty: [true],
            isTwoChair: false
          }
        ],
        [
          {
            id: "a0002",
            isEmpty: [false],
            isTwoChair: false
          },
          {
            id: "a0001",
            isEmpty: [true, true],
            isTwoChair: true
          }
        ]
      ]
    };
  },
  mounted() {
    EventBus.$on("updateId", newSeat => {
      let location = newSeat.location;

      delete newSeat.location;
      this.seats[location[0]][location[1]] = newSeat;
      this.dialog = false;
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

      seats[dragRowIndex][dragColumnIndex] = seats[dropRowIndex][dropColumnIndex];
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
    addRow() {
      this.rowCount++;

      let rowSeats = [];
      for (let columnIndex = 0; columnIndex < this.columnCount; columnIndex++) {
        rowSeats.push({
          id: "",
          isEmpty: false
        });
      }

      this.seats.push(rowSeats);
    },
    removeRow() {
      if (this.rowCount - 1 >= 1) {
        this.rowCount--;
        this.seats.pop();
      }
    },
    addColumn() {
      this.columnCount++;

      let seats = this.seats;
      for (let rowIndex = 0; rowIndex < this.rowCount; rowIndex++) {
        seats[rowIndex].push({
          id: "",
          isEmpty: false
        });
      }
    },
    removeColumn() {
      if (this.columnCount - 1 >= 1) {
        this.columnCount--;
        let seats = this.seats;
        for (let rowIndex = 0; rowIndex < this.rowCount; rowIndex++) {
          seats[rowIndex].pop();
        }
      }
    },
    editSeatId(seat) {
      this.editSeat = seat;
      this.dialog = true;
    }
  }
};
</script>

<style scoped>
.seat {
  /* border: black solid 2px; */
  width: 260px;
  height: 260px;
  color: white;
  font-size: 1.2em;
}

.is-empty {
  background-color: #4caf50;
}

.is-not-empty {
  background-color: #ff5252;
}

.one-chair {
  height: 50% !important;
  width: 100% !important;
}
</style>
