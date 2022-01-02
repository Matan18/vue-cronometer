<script setup lang="ts">
import Form from "./Form.vue";
import Cronometro from "./Cronometro.vue";
import List from "./List/List.vue";
import { ITarefa } from "../types/index";
import { computed, reactive, ref } from "@vue/reactivity";

const list: ITarefa[] = reactive([
  {
    id: 0,
    tempo: "00:01:10",
    tarefa: "Estudar controle de estados em Vue",
    completado: false,
    selecionado: false,
  },
  {
    id: 1,
    tempo: "00:00:10",
    tarefa: "Estudar controle de estados em Vue",
    completado: false,
  },
]);

const isRunning = ref(false);
const selected: ITarefa = reactive({} as ITarefa);
const selectedTime = computed(() => selected.tempo || "00:00:00");

function log(id: number) {
  if (isRunning.value) return;
  list.forEach((item) => {
    if (item.completado) return;
    item.selecionado = item.id === id;
    if (item.selecionado) Object.assign(selected, item);
  });
}
const handleAddTask = (
  data: Omit<ITarefa, "id" | "selecionado" | "completado">
) => list.push({ ...data, id: list.length + 1 });
function handleFinishCountdown() {
  const item = list.find((item) => item.selecionado);
  if (item) {
    item.completado = true;
    item.selecionado = false;
  }
  isRunning.value = false;
}

function startRunning() {
  isRunning.value = true;
}
</script>

<template>
  <div class="container">
    <Form @addTask="handleAddTask" />
    <List :list="list" @changeSelect="log" />
    <Cronometro
      :tempo="selectedTime"
      :isRunning="isRunning"
      @startCountdown="startRunning"
      @finishedCountdown="handleFinishCountdown"
    />
  </div>
</template>

<style scoped>
.container {
  display: grid;
  grid-template-rows: min-content min-content auto;
  grid-template-areas:
    "nova-tarefa"
    "cronometro"
    "tarefas";
  row-gap: 24px;
  min-width: 320px;
  min-height: calc(100vh - 32px);
  width: 100%;
  padding: 32px;
  box-sizing: border-box;
  border-radius: 10px;
  background-color: #171717;
}
@media screen and (min-width: 1280px) {
  .container {
    grid-template-areas:
      "nova-tarefa tarefas"
      "cronometro tarefas";
    column-gap: 64px;
    grid-template-rows: min-content min-content;
    grid-template-columns: 750px 300px;
    justify-content: center;
    align-content: center;
    padding: 64px;
  }
}
</style>
