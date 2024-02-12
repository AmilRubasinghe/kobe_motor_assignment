<template>
  <v-container>
    <PageHeader
      :header="{
        icon: 'mdi-view-list',
        title: 'Todo List',
        subTitle: 'Manage Task List',
      }"
    />
    <v-card flat class="customer-card pa-6">
      <v-row no-gutters class="pa-3 mr-1">
        <v-col class="">
          <v-text-field
            v-model="search"
            prepend-inner-icon="mdi-magnify"
            label="Search"
            single-line
            density="compact"
            flat
            clearable
            hide-details
            class="mr-3"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
        <v-col cols="auto">
          <NuxtLink to="/todos/add">
            <v-btn class="submit-btn px-9">Add ToDo</v-btn>
          </NuxtLink>
        </v-col>
      </v-row>
      <v-data-table
        class="mt-1"
        v-model:search="search"
        v-model:page="page"
        :headers="headers"
        :items="todoItems"
        :sort-by="[
          { key: 'status', order: 'Dsc' },
          { key: 'priority', order: 'asc' },
          { key: 'created_at', order: 'asc' },
        ]"
        :items-per-page="itemsPerPage"
      >
        <template v-slot:item.status="{ item }">
          <v-chip
            :color="
              item.status == 'Done'
                ? 'green'
                : item.status == 'In Progress'
                ? 'orange'
                : 'gray'
            "
            :text="item.status"
            prepend-icon="mdi-circle-medium"
            rounded
            size="small"
          ></v-chip>
        </template>
        <template v-slot:item.tag="{ item }">
          <v-chip
            color="indigo"
            :text="item.tag"
            variant="outlined"
            class="font-weight-medium px-4"
            size="small"
            label
          ></v-chip>
        </template>
        <template v-slot:item.priority="{ item }">
          <p
            :class="
              item.priority == 'High'
                ? 'font-weight-black'
                : item.priority == 'Medium'
                ? 'font-weight-medium'
                : 'font-weight-thin'
            "
          >
            {{ item.priority }}
          </p>
        </template>

        <template v-slot:item.actions="{ item }">
          <NuxtLink :to="`/todos/${item.id}`">
            <v-btn icon size="28" class="me-2">
              <v-icon size="small"> mdi-pencil </v-icon>
            </v-btn>
          </NuxtLink>
          <v-btn icon class="me-2" @click="viewItem(item)" size="28">
            <v-icon size="small"> mdi-eye </v-icon>
          </v-btn>
          <v-btn icon @click="deleteItem(item)" size="28">
            <v-icon size="small"> mdi-delete </v-icon>
          </v-btn>
        </template>
        <template v-slot:bottom>
          <div class="pt-5 pl-3">
            <v-row no-gutters justify-space-between>
              <v-col
                ><div class="table-pagination">
                  Showing page {{ page }} to {{ pageCount }} of
                  {{ todoItems?.length }} entries
                </div>
              </v-col>
              <v-col cols="3">
                <div>
                  <v-pagination
                    size="25"
                    density="compact"
                    v-model="page"
                    active-color="#5A32EA"
                    :length="pageCount"
                  ></v-pagination>
                </div>
              </v-col>
            </v-row>
          </div>
        </template>
      </v-data-table>
    </v-card>
    <DeleteDialog
      v-if="isConfirmNeeded"
      :message="'Are you sure you want to delete permanently?'"
      :show="isConfirmNeeded"
      :btnText="'Delete'"
      @confirm="deleteConfirm"
      @cancel="isConfirmNeeded = false"
    />
    <ToDoViewDialog
      :show="isView"
      :item="viewedItem"
      @cancel="isView = false"
    />
  </v-container>
</template>

<script setup>
import DeleteDialog from "../../components/deleteDialog.vue";
import ToDoViewDialog from "../../components/TodoManagement/ViewTodo.vue";
import PageHeader from "../../components/TopHeader.vue";
import { toast } from "vue3-toastify";

import { ref, onMounted } from "vue";

const todoItems = ref([]);

onMounted(() => {
  const savedTodos = JSON.parse(localStorage.getItem("todos"));

  if (savedTodos) {
    todoItems.value = savedTodos;
  }
});

const isConfirmNeeded = ref(false);

const search = ref("");

const viewedItem = ref({});

const isView = ref(false);

const page = ref(1);

const itemsPerPage = ref(5);

const deleteData = ref({});

const headers = ref([
  {
    title: "Title",
    align: "start",
    key: "title",
    sortable: false,
  },
  {
    title: "Status",
    align: "center",
    key: "status",
  },

  {
    title: "Priority",
    align: "start",
    key: "priority",
  },

  {
    title: "Tag",
    align: "start",
    key: "tag",
    sortable: false,
  },
  {
    title: "Created Date",
    align: "start",
    key: "created_at",
  },

  {
    title: "Action",
    align: "start",
    key: "actions",
    sortable: false,
  },
]);

const pageCount = computed(() => {
  return Math.ceil(todoItems.value.length / itemsPerPage.value);
});

const deleteItem = (item) => {
  isConfirmNeeded.value = true;
  deleteData.value = item;
};

const viewItem = (item) => {
  isView.value = true;
  viewedItem.value = item;
};

const deleteConfirm = () => {
  todoItems.value = todoItems.value.filter(
    (todo) => todo.id !== deleteData.value.id
  );

  saveTodoToLocalStorage();

  isConfirmNeeded.value = false;

  toast.success("Todo deleted.");
};

const saveTodoToLocalStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todoItems.value));
};
</script>
