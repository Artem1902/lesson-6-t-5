<template>
  <h1 class="title-task">Lesson 6 - task 5</h1>
  <div class="wrapper">
    <products-list></products-list>
  </div>
</template>

<script>
import ProductsList from "@/components/ProductsList.vue";

export default {
  name: 'App',
  components: {
    ProductsList,
  },
}
</script>

<style>
@import "assets/styles";

.wrapper {
  max-width: 1200px;
  padding: 10px;
}
.title-task {
  color: orange;
  text-align: center;
  font-size: 28px;
  padding: 20px;
}
</style>
