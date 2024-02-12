<template>
  <div>
    <v-row class="mt-10 px-6">
      <v-col cols="auto d-sm-none d-md-flex d-none d-sm-flex">
        <v-icon
          size="80px"
          class="pr-2 pl-4"
          :icon="headerItems.icon"
          color="secondary"
        >
        </v-icon>
      </v-col>
      <v-col cols="9">
        <h1>{{ headerItems.title }}</h1>
        <p class="text-caption">{{ headerItems.subTitle }}</p>
      </v-col>
    </v-row>
  </div>
</template>
<script setup>
import { ref, defineProps } from "vue";
const props = defineProps({
  header: {
    icon: String,
    title: String,
    subTitle: String,
  },
});
const headerItems = ref(props.header);
</script>
