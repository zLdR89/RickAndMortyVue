<script setup>
import IconRickAndMorty from './icons/IconRickAndMorty.vue'
</script>

<template>
    <div class="heading">
        <div class="logo">
            <IconRickAndMorty />
        </div>
        <div class="headlines">
            <h1 class="green">Rick and Morty</h1>
            <h4>by Florian Zelder</h4>
            <h3>
                simple API search with vite, vue and axios!
            </h3>
        </div>
    </div>
</template>

<style scoped>
.heading {
    display: flex;
    place-items: center;
    place-content: center;
}

.logo {
    margin-right: 2em;
}

h1 {
    font-weight: 500;
    font-size: 2.6rem;
    margin-bottom: -10px;
}

h3 {
    margin-top: 10px;
    font-size: 1.2rem;
}

.headlines h1,
.headlines h4,
.headlines h3 {
    text-align: center;
}

@media (min-width: 1024px) {

    .headlines h1,
    .headlines h4,
    .headlines h3 {
        text-align: left;
    }
}
</style>
