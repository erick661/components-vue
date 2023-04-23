<script setup>
import { ref, computed, onMounted } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadinSpinner.vue';

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref("");

const cambiarFavorito = (title) => {

  favorito.value = title;
}

onMounted(async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    loading.value = false;
  }
})

const next = () => {
  inicio.value += postXpage;
  fin.value += postXpage;
}

const prev = () => {
  inicio.value -= postXpage;
  fin.value -= postXpage;
}

const maxLength = computed(() => {
  return posts.value.length;
});

/* fetch('https://jsonplaceholder.typicode.com/posts').then(res => res.json()).then(data => posts.value = data).finally(() => {
  setTimeout(() => {
    loading.value = false;
  }, 2000)

}); */
</script>
<template>
  <LoadingSpinner v-if="loading"></LoadingSpinner>
  <div class="container" v-else>
    <h1>APP</h1>

    <h2>Mi Post Favorito: {{ favorito }}</h2>

    <PaginatePost class="mb-2" @next="next" @prev="prev" :inicio="inicio" :fin="fin" :max="maxLength" />
    <BlogPost v-for="post in posts.slice(inicio, fin)" :key="post.id" :title="post.title" :id="post.id" :body="post.body"
      @cambiarFavorito="cambiarFavorito" class="mb-2"></BlogPost>

  </div>
</template>

<style></style>
