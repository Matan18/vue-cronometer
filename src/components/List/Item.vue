<script setup lang="ts">
import { ITarefa } from "../../types/index";
const props = defineProps<{
  task: ITarefa;
}>();

const emit = defineEmits(["itemSelect"]);

function onClick() {
  emit("itemSelect", props.task.id);
}
</script>

<template>
  <li
    v-bind:class="{
      itemSelecionado: task.selecionado,
      itemCompletado: task.completado,
    }"
    @click="onClick()"
  >
    <h3>{{ task.tarefa }}</h3>
    <span>{{ task.tempo }}</span>
    <span
      class="concluido"
      v-if="task.completado"
      aria-label="icone de concluido"
    ></span>
  </li>
</template>

<style>
li {
  background-color: #4d4d4d;
  border-radius: 8px;
  box-shadow: 2px 4px 4px #0000009f;
  padding: 12px;
  margin-bottom: 8px;
  position: relative;
  cursor: pointer;
}
h3 {
  margin-bottom: 8px;
  word-break: break-all;
}

span {
  color: #d0d0d0;
}

@media screen and (min-width: 1280px) {
  li {
    font-size: 1.8rem;
  }
}

.itemSelecionado {
  background-color: #292929;
  box-shadow: 2px 4px 4px #0000009f inset;
}

.itemCompletado {
  background: #566f42;
  cursor: auto;
}
.concluido {
  display: block;
  background-image: url("/assets/check-mark.svg");
  background-repeat: no-repeat;
  background-size: 38px 38px;
  position: absolute;
  top: 50%;
  right: 12px;
  transform: translateY(-50%);
  width: 42px;
  height: 43px;
}
</style>