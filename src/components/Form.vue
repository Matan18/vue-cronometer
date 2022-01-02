<template>
  <form @submit="createTask">
    <fieldset>
      <label htmlFor="tarefa">Adicione um novo estudo</label>
      <input
        type="text"
        name="tarefa"
        id="tarefa"
        placeholder="O que você quer estudar?"
        required
      />
    </fieldset>
    <fieldset>
      <label htmlFor="tempo">Tempo</label>
      <input
        type="time"
        step="1"
        name="tempo"
        id="tempo"
        min="00:00:00"
        max="01:30:00"
        required
      />
    </fieldset>
    <button type="submit">Adicionar</button>
  </form>
</template>

<script setup lang="ts">
const emit = defineEmits<{
  (event: "addTask", data: { tempo: string; tarefa: string }): void;
}>();
function createTask(event: any) {
  event.preventDefault();
  const formData = new FormData(event.target);
  const data: { [key: string]: string } = {};
  formData.forEach((value, key) => (data[key] = `${value}`));
  emit("addTask", data as { tempo: string; tarefa: string });
}
</script>

<style scoped>
form {
  width: 100%;
  display: flex;
  flex-direction: column;
  grid-area: nova-tarefa;
  background-color: #7687a1;
  border-radius: 10px;
  box-shadow: 2px 4px 4px #0000009f;
  padding: 12px;
}
form fieldset {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin-bottom: 16px;
}
label {
  margin-bottom: 8px;
  font-size: 1.25rem;
}

input {
  width: 100%;
  padding: 8px 12px 4px;
  box-sizing: border-box;
  border: unset;
  border-radius: 5px;
  background-color: #5d677c;
  box-shadow: 0px 2px 4px #2d2b2b9f inset;
}
form fieldset input::placeholder {
  color: #bfbfbf;
}

form button {
  align-self: center;
  width: 150px;
  padding: 16px;
  color: #272626;
  font-size: 1.25rem;
  background-color: #88bcd1;
  border-radius: 10px;
  box-shadow: 2px 4px 4px #0000009f;
  cursor: pointer;
}
form button:active {
  background-color: #7ca6b7;
  box-shadow: 2px 2px 4px #0000009f inset;
}

@media screen and (min-width: 1280px) {
  form {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    font-size: 2.25rem;
    padding: 24px;
    box-sizing: border-box;
  }
  form fieldset {
    width: calc(60% - 12px);
  }
  form fieldset:last-of-type {
    width: 40%;
  }

  label {
    font-size: 2rem;
  }

  input {
    height: 100%;
    font-size: 1.75rem;
  }

  button {
    grid-column-start: span 2;
    justify-self: center;
    width: 200px;
    font-size: 2.25rem;
  }
}
</style>