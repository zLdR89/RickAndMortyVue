<script setup>
const props = defineProps({
  item: Object
});
</script>

<template>
  <img :src="item.image" />
  <div class="suggestion-info">
    <h3>{{ item.name }}</h3>
    <span>{{ item.status }}</span><br />
    <span>Episodes: {{ item.episode.length }}</span>
    <br />
    <a :href="item.location.url">{{ item.location.name }} </a>
  </div>
</template>

<style scoped>
h3 {
  font-weight: bold;
}

div.suggestion-info {
  height: 100px;
  display: inline-block;
  margin: 0 20px;
  vertical-align: top;
}

img {
  width: 100px;
  height: 100px;
  display: inline-block;
  border-radius: 24px;
}
</style>


