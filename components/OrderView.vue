<template>
  <div class="user mb-2 py-2">
    <span class="h5 mt-2">{{ student_no }} - Order No. {{ order_no }}</span>
    <div class="button-section">
        <button class="btn btn-secondary text-dark">View</button>
        <button class="btn btn-warning" @click="deleteUser">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
    props: ["student_no", "order_no"]
}
</script>

<style>

</style>