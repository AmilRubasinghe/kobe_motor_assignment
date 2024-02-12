<template>
  <v-container fluid class="fill-height">
    <v-layout wrap align-center>
      <v-row justify="center">
        <v-progress-circular
          :size="100"
          :width="7"
          indeterminate
          color="primary"
        ></v-progress-circular>
      </v-row>
    </v-layout>
  </v-container>
</template>

<script setup>
await navigateTo("/todos");
</script>
