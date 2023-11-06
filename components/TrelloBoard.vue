<template>
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      class="flex gap-4 overflow-x-auto items-start"
      :animation="150"
      handle=".drag-handle"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 column rounded p-5 min-w-[250px]">
          <header class="font-bold mb-4 flex items-center">
            <DragHandle></DragHandle>
            <input
              type="text"
              v-model="column.title"
              class="title-input bg-transperant focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
            />
            <button
              class="pl-4"
              @click="columns = columns.filter((c) => c.id !== column.id)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                x="0px"
                y="0px"
                width="20"
                height="20"
                viewBox="0 0 48 48"
              >
                <linearGradient
                  id="hbE9Evnj3wAjjA2RX0We2a_OZuepOQd0omj_gr1"
                  x1="7.534"
                  x2="27.557"
                  y1="7.534"
                  y2="27.557"
                  gradientUnits="userSpaceOnUse"
                >
                  <stop offset="0" stop-color="#f44f5a"></stop>
                  <stop offset=".443" stop-color="#ee3d4a"></stop>
                  <stop offset="1" stop-color="#e52030"></stop>
                </linearGradient>
                <path
                  fill="url(#hbE9Evnj3wAjjA2RX0We2a_OZuepOQd0omj_gr1)"
                  d="M42.42,12.401c0.774-0.774,0.774-2.028,0-2.802L38.401,5.58c-0.774-0.774-2.028-0.774-2.802,0	L24,17.179L12.401,5.58c-0.774-0.774-2.028-0.774-2.802,0L5.58,9.599c-0.774,0.774-0.774,2.028,0,2.802L17.179,24L5.58,35.599	c-0.774,0.774-0.774,2.028,0,2.802l4.019,4.019c0.774,0.774,2.028,0.774,2.802,0L42.42,12.401z"
                ></path>
                <linearGradient
                  id="hbE9Evnj3wAjjA2RX0We2b_OZuepOQd0omj_gr2"
                  x1="27.373"
                  x2="40.507"
                  y1="27.373"
                  y2="40.507"
                  gradientUnits="userSpaceOnUse"
                >
                  <stop offset="0" stop-color="#a8142e"></stop>
                  <stop offset=".179" stop-color="#ba1632"></stop>
                  <stop offset=".243" stop-color="#c21734"></stop>
                </linearGradient>
                <path
                  fill="url(#hbE9Evnj3wAjjA2RX0We2b_OZuepOQd0omj_gr2)"
                  d="M24,30.821L35.599,42.42c0.774,0.774,2.028,0.774,2.802,0l4.019-4.019	c0.774-0.774,0.774-2.028,0-2.802L30.821,24L24,30.821z"
                ></path>
              </svg>
            </button>
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
            <NewTask @add-task="column.tasks.push($event)"></NewTask>
          </footer>
        </div>
      </template>
    </draggable>
    <button
      @click="createColumn"
      class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
    >
      + Add another column
    </button>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from "nanoid";
import type { Column, Task } from "~/types";
import draggable from "vuedraggable";

const columns = useLocalStorage("trelloBoard", [{}]);

function createColumn() {
  columns.value.push({
    title: "",
    id: nanoid(),
    tasks: [],
  });
  nextTick(() =>
    (
      document.querySelector(
        ".column:last-of-type .title-input"
      ) as HTMLInputElement
    ).focus()
  );
}

const alt = useKeyModifier("Alt");
</script>

<style scoped></style>
