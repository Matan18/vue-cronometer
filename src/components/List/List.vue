<template>
  <aside>
    <h2>Estudos do dia</h2>
    <ul>
      <li v-for="item in props.list" :key="item.id">
        <Item :task="item" @itemSelect="log" />
      </li>
    </ul>
  </aside>
</template>

<script setup lang="ts">
import { ITarefa } from "../../types/index";
import Item from "./Item.vue";
const props = defineProps<{
  list: ITarefa[];
}>();

const emit = defineEmits(["changeSelect"]);

function log(id: number) {
  emit("changeSelect", id);
}
</script>

<style>
aside {
  grid-area: tarefas;
  height: 100%;
}
h2 {
  font-size: 1.25rem;
  margin-bottom: 12px;
}

ul {
  max-height: 350px;
  overflow-y: scroll;
  scrollbar-width: thin;
}

@media screen and (min-width: 1280px) {
  h2 {
    text-align: center;
    font-size: 2.25rem;
    margin-bottom: 24px;
  }

  ul {
    overflow: auto;
    max-height: 500px;
  }
}
</style>