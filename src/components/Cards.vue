<script setup>
import { ref } from "vue";
import Card from "./Card.vue";
import InfiniteScroller from "./InfiniteScroller.vue";
let items = ref([]);
let limit = 100;
let offset = 0;
const loadItems = async () => {
  const newItems = await generateData(limit, offset);
  items.value = [...items.value, ...newItems];
  ++offset;
};
const generateData = (limit, offset, delay = 200) => {
  return new Promise((resolve) => {
    const newData = Array(limit)
      .fill(0)
      .map((_, index) => index + offset * limit + 1);
    setTimeout(resolve, delay, newData);
  });
};
</script>

<template>
  <InfiniteScroller class="cards" @infinite="loadItems">
    <Card v-for="item in items" :value="item" :key="item"></Card>
  </InfiniteScroller>
</template>

<style scoped>
.cards {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
  padding: 1rem;
  box-sizing: border-box;
}
</style>
