<template>
  <div class="clientItem my-5 mx-4 p-4 border-light">
      <div class="clientInfo mt-2">
          <span class="h3">{{ foodName }} - <b>P{{ foodPrice }}</b></span>
      </div>
      <button class="btn btn-lg btn-secondary" @click="$emit('order', foodId)">Order This!</button>
  </div>
</template>

<script>
export default {
    props: ["foodId", "foodName", "foodPrice"]
}
</script>

<style>
    .clientItem {
        display: flex;
        justify-content: space-between;
        background-color: #F6C492;
        box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.25);
    }
</style>