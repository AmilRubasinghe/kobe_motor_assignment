<template>
  <v-container>
    <v-card flat class="customer-card pa-6 mt-9">
      <v-form ref="form" v-model="isFormValid">
        <v-row class="my-3">
          <v-col class="ma-0 pa-0">
            <v-text-field
              v-model="data.title"
              label="Title *"
              :rules="[(v) => !!v || 'Title is required']"
              required
              :readonly="props.type == 'isView'"
              class="mx-2"
              variant="outlined"
            ></v-text-field>
          </v-col>
          <v-col class="ma-0 pa-0">
            <v-text-field
              v-model="data.tag"
              label="Tag *"
              :readonly="props.type == 'isView'"
              :rules="[rules.required, rules.maxCounter]"
              required
              class="mx-2"
              variant="outlined"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row class="my-3">
          <v-col class="ma-0 pa-0">
            <v-select
              :items="priorityItems"
              :readonly="props.type == 'isView'"
              v-model="data.priority"
              label="Priority *"
              :rules="[(v) => !!v || 'Priority is required']"
              required
              class="mx-2"
              variant="outlined"
            ></v-select>
          </v-col>
          <v-col class="ma-0 pa-0">
            <v-select
              :items="statusItems"
              :readonly="props.type == 'isView'"
              v-model="data.status"
              label="Status *"
              :rules="[(v) => !!v || 'Status is required']"
              required
              class="mx-2"
              variant="outlined"
            ></v-select>
          </v-col>
        </v-row>
        <v-row class="my-3">
          <v-textarea
            clearable
            v-model="data.description"
            :readonly="props.type == 'isView'"
            label="Description *"
            :rules="[(v) => !!v || 'Description is required']"
            required
            class="mx-2"
            variant="outlined"
          ></v-textarea>
        </v-row>
      </v-form>
      <v-card-actions v-show="props.type != 'isView'" class="mt-8">
        <v-row justify="end">
          <NuxtLink to="/todos">
            <v-btn class="mr-2 reset-btn px-10">Back</v-btn>
          </NuxtLink>
          <v-btn @click="todoSubmitted()" class="submit-btn px-10">{{
            props.type == "isAdd" ? "Create" : "Edit"
          }}</v-btn>
        </v-row>
      </v-card-actions>
    </v-card>
  </v-container>
</template>
<script setup>
import { toast } from "vue3-toastify";

import { ref, onMounted } from "vue";

import { defineProps } from "vue";

const priorityItems = ref(["Low", "Medium", "High"]);

const statusItems = ref(["Done", "In Progress", "Not Started"]);

const data = ref({});

const todoItems = ref([]);

const form = ref(null);

const props = defineProps({
  type: String,
  id: Number | String,
});

const rules = ref({
  required: (value) => !!value || "Tag is required.",
  maxCounter: (value) => value.length <= 10 || "Max letters count is 10",
});

const isFormValid = ref(false);

onMounted(() => {
  const savedTodos = JSON.parse(localStorage.getItem("todos"));

  if (savedTodos) {
    todoItems.value = savedTodos;
  }

  if (props.type != "isAdd" && props.id) {
    data.value = todoItems.value.find((todo) => todo.id == props.id);
  }
});

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

// Submit transaction
const todoSubmitted = async () => {
  form.value.validate();

  if (!isFormValid.value) return;

  if (props.type == "isEdit") {
    todoItems.value = todoItems.value.filter(
      (todo) => todo.id !== data.value.id
    );
  }

  const val = {
    id: props.type == "isAdd" ? generateUniqueId() : data.value.id,
    title: data.value.title,
    description: data.value.description,
    priority: data.value.priority,
    status: data.value.status,
    created_at: props.type == "isAdd" ? new Date() : data.value.created_at,
    tag: data.value.tag,
  };

  todoItems.value.push(val);

  saveTodoToLocalStorage();

  toast.success(
    props.type == "isAdd"
      ? "Todo added successfully"
      : "Todo updated successfully"
  );

  await navigateTo("/todos");
};

// Save todo to local storage
const saveTodoToLocalStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todoItems.value));
};
</script>
