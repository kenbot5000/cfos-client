<template>
  <div class="user mb-2 py-2">
    <span class="h5 mt-2">{{ username }}</span>
    <div class="button-section">
      <button class="btn btn-secondary text-dark" @click="$emit('edit-user', id)">Edit</button>
      <button class="btn btn-warning">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "User",
  props: ["username", "id"],
}
</script>

<style>
  .user {
    display: flex;
    justify-content: space-between;
  }
</style>