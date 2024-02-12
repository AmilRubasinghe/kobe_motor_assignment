<template>
  <v-container>
    <PageHeader
      :header="{
        icon: 'mdi-forum-plus-outline',
        title: 'Create Todo',
        subTitle: 'New Task details',
      }"
    />
    <TodoForm :type="'isAdd'" />
  </v-container>
</template>
<script setup>
import PageHeader from "../../components/TopHeader.vue";

import TodoForm from "../../components/TodoManagement/TodoForm.vue";

import { ref } from "vue";

const todo = ref([]);
</script>
