<script setup>
import { VueFlow, useVueFlow } from '@vue-flow/core';
import { ref, watch, onMounted } from 'vue';
import { useDebouncedRefHistory } from '@vueuse/core';

const initial = [
  {
    id: '1',
    type: 'input',
    label: 'Node 1',
    position: { x: 250, y: 5 },
    class: 'light',
  },
  {
    id: '2',
    label: 'Node 2',
    position: { x: 100, y: 100 },
    class: 'light',
  },
  {
    id: '3',
    label: 'Node 3',
    position: { x: 400, y: 100 },
    class: 'light',
  },
  {
    id: '4',
    label: 'Node 4',
    position: { x: 400, y: 200 },
    class: 'light',
  },
  { id: 'e1-2', source: '1', target: '2' },
  { id: 'e1-3', source: '1', target: '3' },
  { id: 'e3-4', source: '3', target: '4' },
];

const { onConnect, addEdges, toObject } = useVueFlow();

const elements = ref([...toObject().nodes, ...toObject().edges]);

const { history, undo, redo } = useDebouncedRefHistory(elements, {
  deep: true,
  debounce: 500,
});

watch(toObject, (v) => console.log(v));

onMounted(() => {
  elements.value = initial;
});

onConnect((params) => addEdges([params]));
</script>

<template>
  <VueFlow v-model="elements" :fit-view-on-init="true">
    <div
      style="
        position: absolute;
        right: 10px;
        top: 10px;
        z-index: 4;
        display: flex;
        gap: 0.25rem;
      "
    >
      <button @click="undo()">undo</button>
      <button @click="redo()">redo</button>
    </div>
  </VueFlow>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
