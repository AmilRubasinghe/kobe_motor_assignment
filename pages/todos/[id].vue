<template>
  <v-container>
    <PageHeader
      :header="{
        icon: 'mdi-text-box-edit-outline',
        title: 'Edit Todo',
        subTitle: 'Change existing task details',
      }"
    />
    <TodoForm :type="'isEdit'" :id="id" />
  </v-container>
</template>
<script setup>
import PageHeader from "../../components/TopHeader.vue";

import TodoForm from "../../components/TodoManagement/TodoForm.vue";

import { ref } from "vue";

const route = useRoute();

const id = ref(route.params.id);
</script>
