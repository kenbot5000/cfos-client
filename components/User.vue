<template>
  <div class="user mb-2 py-2">
    <span class="h5 mt-2">{{ username }}</span>
    <div class="button-section">
      <button class="btn btn-secondary text-dark" @click="$emit('edit-user', userid)">Edit</button>
      <button class="btn btn-warning" @click="deleteUser">Delete</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "User",
  props: ["username", "userid"],
  data() {
    return {
      dataUserId: this.userid
    }
  },
  methods: {
    deleteUser() {
      try {
        const res = axios.delete(`http://localhost:5000/user/${this.dataUserId}`);
        this.$emit("delete-user", this.dataUserId);
      } catch(err) {
        console.log(err);
      }
    }
  }
}
</script>

<style>
  .user {
    display: flex;
    justify-content: space-between;
  }
</style>