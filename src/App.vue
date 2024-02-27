<script setup>
import { ref, computed } from "vue";

const name = "Vue 3";
const styleColor = "color:blue";
const arrayColores = ["blue", "red", "green"];
const arrayFavoritos = ref([]);
let activo = ref(true);
const handleClick = () => {
  activo.value = activo.value ? 0 : 1;
};
const counter = ref(0);
const increment = () => {
  counter.value++;
};
const decrement = () => {
  counter.value--;
};
const reset = () => {
  counter.value = 0;
};
const add = () => {
  arrayFavoritos.value.push(counter.value);
};
const clear = () => {
  arrayFavoritos.value = [];
};
const classCounter = computed(() => {
  if (counter.value === 0) {
    return "zero";
  } else if (counter.value > 0) {
    return "positive";
  } else {
    return "negative";
  }
});
const counterExiste = computed(() => {
  const found = arrayFavoritos.value.find((e) => e === counter.value);
  return found || found === 0;
});
</script>
<template>
  <div class="container text-center mt-3">
    <h1>Hola {{ name.toUpperCase() }} dinámico</h1>
    <br />
    <h2 :style="styleColor">Soy Oscar</h2>
    <br />
    <div :style="`color:${activo ? arrayColores[2] : arrayColores[1]}`">
      <button @click="handleClick">📍</button>
      {{ activo ? "Estoy activo" : "Estoy inactivo" }}
    </div>
    <br />
    <h2 :class="classCounter">{{ counter }}</h2>
    <br />
    <div class="btn-group">
      <button class="btn btn-success" :disabled="!activo" @click="increment">
        + Increment
      </button>
      <button class="btn btn-danger" :disabled="!activo" @click="decrement">
        - Decrement
      </button>
      <button class="btn btn-secondary" :disabled="!activo" @click="reset">
        Reset
      </button>
      <button class="btn btn-primary" :disabled="counterExiste" @click="add">
        Add
      </button>
      <button class="btn btn-warning" :disabled="activo" @click="clear">
        Clear
      </button>
    </div>
    <br />
    <ul class="list-group mt-2">
      <li
        class="list-group-item"
        v-for="(num, index) in arrayFavoritos"
        :key="index"
      >
        {{ num }}
      </li>
    </ul>
  </div>
</template>
<style>
h1 {
  color: red;
}
.positive {
  color: green;
}
.negative {
  color: red;
}
.zero {
  color: peru;
}
</style>
