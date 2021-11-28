<template>
  <div className="cronometro">
    <p className="titulo">Escolha um card e inicie o cronômetro</p>
    <div className="relogioWrapper">
      <Relogio v-bind:totalSegundos="time" />
    </div>
    <button :disabled="isRunning" @click="iniciarCronometro">Começar</button>
  </div>
</template>

<script setup lang="ts">
import Relogio from "./Relogio.vue";
import { delay, timeToSeconds } from "../utils/date";
import { computed, ref } from "@vue/reactivity";
import { watch, watchEffect } from "@vue/runtime-core";
interface ICronometro {
  tempo: string;
  isRunning: boolean;
}
const props = defineProps<ICronometro>();
const emit = defineEmits<{
  (event: "finishedCountdown"): void;
  (event: "startCountdown"): void;
}>();

const time = ref(0);
const isDisabled = ref(false);
watchEffect(() => props.isRunning);
watchEffect(() => (time.value = timeToSeconds(props.tempo)));
async function iniciarCronometro() {
  emit("startCountdown");
  while (time.value > 0) {
    await delay();
    time.value--;
  }
  emit("finishedCountdown");
}
</script>

<style>
.cronometro {
  display: flex;
  flex-direction: column;
  align-items: center;
  grid-area: cronometro;
}
.relogioWrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  box-sizing: border-box;
  border-radius: 10px;
  padding: 16px 12px;
  margin-bottom: 24px;
  box-shadow: 2px 4px 4px #0000009f;
  font-size: 5rem;
  background-color: #7687a1;
}

.titulo {
  font-size: 2rem;
}

button {
  width: 150px;
  padding: 16px;
  color: #272626;
  font-size: 1.25rem;
  background-color: #88bcd1;
  border-radius: 10px;
  box-shadow: 2px 4px 4px #0000009f;
  cursor: pointer;
}
button:active {
  background-color: #7ca6b7;
  box-shadow: 2px 2px 4px #0000009f inset;
  cursor: auto;
}

@media screen and (min-width: 1280px) {
  .relogioWrapper {
    font-size: 15rem;
  }

  p {
    font-size: 2rem;
  }

  button {
    grid-column-start: span 2;
    justify-self: center;
    width: 200px;
    font-size: 2.25rem;
  }
}
</style>