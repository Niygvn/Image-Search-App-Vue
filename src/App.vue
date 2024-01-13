<template>
  <div>
    <h1>Image Search App</h1>
    <form @submit.prevent="searchImages">
      <input
        type="text"
        placeholder="Search for images ..."
        id="search-input"
        v-model="image"
      />
      <button id="search-button" type="submit">Get Images</button>
    </form>
    <div class="search-results">
      <div
        class="search-result"
        v-for="(result, index) in results"
        :key="index"
      >
        <img :src="result.urls.small" :alt="result.alt_description" />
        <a :href="result.links.html" target="_blank">{{
          result.alt_description
        }}</a>
      </div>
    </div>
    <button id="load-more" v-if="showLoadMore" @click="loadMore">
      Load More
    </button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const apiKey = "jYu28ah5FBYuv1Ddj_tRodorJe3NEtmqLnplddTWQEU";
const image = ref("");
const results = ref([]);
const page = ref(1);
const showLoadMore = ref(false);

const searchImages = async () => {
  const url = `https://api.unsplash.com/search/photos?page=${page.value}&query=${image.value}&client_id=${apiKey}`;
  const response = await fetch(url);
  const data = await response.json();
  results.value =
    page.value === 1
      ? data.results.slice(0, -1)
      : [...results.value, ...data.results.slice(0, -1)];
  page.value++;
  if (page.value > 1) {
    showLoadMore.value = true;
  }
};

const loadMore = () => {
  searchImages();
};
</script>
