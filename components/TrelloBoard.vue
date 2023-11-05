<template>
  <div>
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      class="flex gap-4 overflow-x-auto items-start"
      :animation="150"
      handle=".drag-handle"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="bg-gray-200 column rounded p-5 min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle></DragHandle>
            {{ column.title }}
          </header>
          <draggable
            v-model="column.tasks"
            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
            item-key="id"
            :animation="150"
            handle=".drag-handle"
          >
            <template #item="{ element: task }: { element: Task }">
              <TrelloTask
                @delete-task="
                  column.tasks = column.tasks.filter((t) => t.id !== $event)
                "
                :task="task"
              ></TrelloTask>
            </template>
          </draggable>

          <footer>
            <NewTask
              @add-task="
                (e) => {
                  column.tasks.push(e);
                }
              "
            ></NewTask>
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from "nanoid";
import type { Column, Task } from "~/types";
import draggable from "vuedraggable";

const columns = ref<Column[]>([
  {
    title: "Washing dishes",
    id: nanoid(),
    tasks: [
      {
        id: nanoid(),
        title: "Wash the dishes",
        createAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Wash the dishes twice :D",
        createAt: new Date(),
      },
    ],
  },

  {
    title: "Take a shower",
    id: nanoid(),
    tasks: [],
  },
]);

const alt = useKeyModifier("Alt");
</script>

<style scoped></style>
