<template>
  <div class="text-center">
    <v-dialog
      v-model="show"
      max-width="650px"
      transition="dialog-transition"
      persistent
    >
      <v-card class="py-5">
        <v-container>
          <v-row justify="center">
            <v-card-title class="headline text-center">
              <span class="delete-head">{{ btnText }} Confirmation</span>
            </v-card-title>
          </v-row>
          <v-row justify="center" class="delete-row">
            <v-card-text class="text-center">
              <span class="delete-body"> {{ message }} </span>
            </v-card-text>
          </v-row>
          <v-card-actions class="mt-8">
            <v-row justify="center">
              <v-btn @click="$emit('cancel')" class="mr-2 reset-btn px-7"
                >Cancel</v-btn
              >
              <v-btn @click="$emit('confirm')" class="submit-btn px-7">{{
                btnText
              }}</v-btn>
            </v-row>
          </v-card-actions>
        </v-container>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
import { defineProps, ref } from "vue";

const props = defineProps({
  show: {
    type: Boolean,
    required: true,
  },
  message: {
    type: String,
    required: true,
  },
  btnText: {
    type: String,
    required: true,
  },
});

const show = ref(props.show);

const message = ref(props.message);

const btnText = ref(props.btnText);

defineEmits(["confirm", "cancel"]);
</script>

<style></style>
