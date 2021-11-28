<template>
  <span className="relogioNumero">{{ timeData.minutoEsquerdo }}</span>
  <span className="relogioNumero">{{ timeData.minutoDireito }}</span>
  <span className="relogioDivisao">:</span>
  <span className="relogioNumero">{{ timeData.segundoEsquerdo }}</span>
  <span className="relogioNumero">{{ timeData.segundoDireito }}</span>
</template>

<script setup lang="ts">
import { computed } from "@vue/reactivity";
import { timeToSeconds } from "../utils/date";

interface IRelogio {
  totalSegundos: number;
}
const props = defineProps<IRelogio>();

const timeData = computed(() => {
  const totalSegundos = props.totalSegundos;
  const minutos = ("0" + Math.floor(totalSegundos / 60)).slice(-2);
  const segundosRestantes = ("0" + (totalSegundos % 60)).slice(-2);
  const [minutoEsquerdo, minutoDireito] = minutos.split("");
  const [segundoEsquerdo, segundoDireito] = segundosRestantes.split("");
  return {
    minutoEsquerdo,
    minutoDireito,
    segundoEsquerdo,
    segundoDireito,
  };
});
</script>

<style>
.relogioNumero {
  background-color: #5d677c;
  box-shadow: 2px 2px 4px #2b2b2b inset;
  height: 1.1em;
  text-align: center;
  width: 3rem;
  border-radius: 10px;
}
@media screen and (min-width: 1280px) {
  .relogioNumero {
    font-size: 10.8rem;
    width: 9rem;
  }

  .relogioDivisao {
    height: 4.2rem;
  }
}
@media screen and (min-width: 1280px) {
  .relogioDivisao {
    height: 1em;
  }
}
</style>