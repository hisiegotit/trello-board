<template>
  <div>
    <textarea
      v-model="title"
      @focus="focused = true"
      @blur="focused = false"
      @keyup.enter="createTask"
      @keydown.tab="createTask"
      class="focus:shadow bg-gray-200 resize-none rounded w-full p-2"
      :class="{
        'h-10': !focused,
        'h-20 bg-white': focused,
      }"
      :placeholder="
        focused ? 'Enter a title for this card...' : '+ Add a card...'
      "
    />
  </div>
</template>

<script setup lang="ts">
import type { Task } from "~/types";
import { nanoid } from "nanoid";
const emit = defineEmits<{
  (e: "addTask", task: Task): void;
}>();

const focused = ref(false);
const title = ref("");
function createTask(e: Event) {
  if (title.value.trim()) {
    e.preventDefault();
    emit("addTask", {
      id: nanoid(),
      title: title.value,
      createAt: new Date(),
    } as Task);
  }
  title.value = "";
}
</script>

<style scoped></style>
