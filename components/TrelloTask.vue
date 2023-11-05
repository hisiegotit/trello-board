<template>
  <div
    :title="task.createAt.toLocaleString()"
    class="task bg-white p-2 mb-2 rounded shadow-sm max-w-[250px] flex"
    @focus="focused = true"
    @blur="focused = false"
    tabindex="0"
  >
    <DragHandle></DragHandle>
    {{ task.title }}
  </div>
</template>

<script setup lang="ts">
import type { Task } from "~/types";
const props = defineProps<{ task: Task }>();

const focused = ref(false);

const emit = defineEmits<{ (e: "deleteTask", payload: string): void }>();

onKeyStroke("Backspace", (e) => {
  if (focused.value) {
    emit("deleteTask", props.task.id);
  }
});
</script>

<style lang="scss">
.sortable-drag .task {
  transform: rotate(5deg);
}
.sortable-ghost .task {
  position: relative;
}

.sortable-ghost .task::after {
  content: "";
  @apply absolute top-0 left-0 w-full h-full bg-slate-300  rounded shadow-sm;
}

.task:focus,
.task:focus-visible {
  outline: gray auto 1px;
  @apply outline-gray-400;
}
</style>
