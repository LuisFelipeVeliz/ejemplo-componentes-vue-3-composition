<script setup>
import { computed, onMounted, ref } from 'vue';
import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

const posts = ref([]);
const postsXPage = 10;
const inicio = ref(0);
const fin = ref(postsXPage);
const favorito = ref("");
const loading = ref(true);

const cambiarFavorito = (title) => {
  favorito.value = title;
}

const next = () => {
  inicio.value = inicio.value + postsXPage;
  fin.value = fin.value + postsXPage;
}

const prev = () => {
  inicio.value = inicio.value - postsXPage;
  fin.value = fin.value - postsXPage;
}

/* 
onMounted(() => {
  fetchData();
});
 */

/* 
fetch("https://jsonplaceholder.typicode.com/posts")
  .then(res => res.json())
  .then(data => {
    posts.value = data;
  })
  .catch(error => {
    console.log("error");
  })
  .finally(() => {
    setTimeout(() => {
      loading.value = false
    }, 2000);
  }); 
*/

const fetchData = async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) { 
    console.log(error);
  } finally {
    setTimeout(() => {
      loading.value = false
    }, 2000);
  }
}

fetchData();

const total = computed(() => posts.value.length);
</script>

<template>
  <LoadingSpinner v-if="loading"></LoadingSpinner>
  <div class="container pt-3" v-else>
    <h1>APP_</h1>
    <h2>Mi Post Favorito: {{ favorito }}</h2>

    <PaginatePost
      @next="next"
      @prev="prev"
      :inicio="inicio"
      :fin="fin"
      :total="total"
    ></PaginatePost>

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