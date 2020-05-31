<template>
  <v-dialog v-model="dialog" max-width="290">
    <v-card>
      <v-card-title class="headline">Check Your Action</v-card-title>

      <v-card-text class="pb-0">確認要執行 {{ action }} 嗎？</v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" text @click="closeDialog">取消</v-btn>
        <v-btn color="green darken-1" text @click="checkAction">
          儲存
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import EventBus from "@/plugins/event-bus";

export default {
  props: {
    action: String,
    dialog: Boolean
  },
  watch: {
    dialog() {
      if (this.dialog) {
        document.addEventListener(
          "keydown",
          e => {
            if (e.keyCode == 27) {
              this.closeDialog();
            }
          },
          {
            once: true
          }
        );

        document.addEventListener(
          "keydown",
          e => {
            if (e.keyCode == 13) {
              this.checkAction();
            }
          },
          {
            once: true
          }
        );
      }
    }
  },
  methods: {
    closeDialog() {
      EventBus.$emit("closeDialog");
    },
    checkAction() {
      EventBus.$emit("confirmAction", this.action);
    }
  }
};
</script>
