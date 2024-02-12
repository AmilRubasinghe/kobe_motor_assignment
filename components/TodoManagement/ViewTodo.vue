<template>
  <div class="text-center">
    <v-dialog
      v-model="props.show"
      max-width="650px"
      transition="dialog-transition"
      persistent
    >
      <v-card class="py-5">
        <v-container>
          <v-row justify="space-between" class="mb-n9 pa-3">
            <v-card-title class="ml-7">
              <span class="dialog-head">View Todo Details</span>
            </v-card-title>
            <v-btn
              @click="$emit('cancel')"
              variant="flat"
              size="large"
              class="mt-n8"
              icon="mdi-close"
            ></v-btn>
          </v-row>
          <TodoForm :type="'isView'" :id="props.item.id" />
        </v-container>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import { defineProps, ref } from "vue";

import TodoForm from "./TodoForm.vue";

const props = defineProps({
  show: Boolean,
  item: Object,
});
defineEmits(["cancel"]);
const show = ref(props.show);
</script>

<style></style>
