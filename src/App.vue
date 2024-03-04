<script setup>
import { ref, computed, onMounted } from "vue";
import ButtonCounter from "./components/ButtonCounter.vue";
import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

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
const counterExiste = computed(() => {
  const found = arrayFavoritos.value.find((e) => e === counter.value);
  return found || found === 0;
});
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(false);

const maxLength = computed(() => posts.value.length);
const next = () => {
  inicio.value += postXpage;
  fin.value += postXpage;
};
const prev = () => {
  inicio.value += -postXpage;
  fin.value += -postXpage;
};

const favorito = ref("");
const cambiarFavorito = (title) => {
  favorito.value = title;
};
const posts = ref([]);

onMounted(() => {});
const fetchData = async () => {
  loading.value = true;
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    loading.value = false;
  }
};
fetchData();
/* fetch("https://jsonplaceholder.typicode.com/posts")
    .then((res) => res.json())
    .then((data) => (posts.value = data))
    .catch((e) => console.log(e))
    .finally(() => (loading.value = false)); */
</script>
<template>
  <LoadingSpinner v-if="loading" />
  <div v-else class="container text-left mt-3">
    <h1>Hola {{ name.toUpperCase() }} dinámico</h1>
    <br />
    <h2 :style="styleColor">Mi Post Favorito: {{ favorito }}</h2>
    <br />
    <div :style="`color:${activo ? arrayColores[2] : arrayColores[1]}`">
      <button @click="handleClick">📍</button>
      {{ activo ? "Estoy activo" : "Estoy inactivo" }}
    </div>
    <br />
    <ButtonCounter :counterValue="counter" />
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
    <PaginatePost
      class="mb-2"
      @next="next"
      @prev="prev"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
    />
    <BlogPost
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :id="post.id"
      :title="post.title"
      :body="post.body"
      @cambiarFavorito="cambiarFavorito"
    ></BlogPost>
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
