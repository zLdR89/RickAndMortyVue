<script setup>
const props = defineProps({
  item: Object
});
</script>

<template>
  <img :src="item.image" />
  <h1>{{ item.name }}</h1>
  <h3>{{ item.status }}</h3>
  <span>Episodes: {{ item.episode.length }}</span>
  <br /><br />
  <a :href="item.location.url">{{ item.location.name }} </a>
</template>

<style scoped>
img {
  width: 150px;
  height: 150px;
  float: left;
  margin-right: 40px;
  border-radius: 24px;
}
</style>


