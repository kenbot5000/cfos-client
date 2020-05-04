<template>
  <nav class="navbar navbar-expand-lg navbar-fixed navbar-dark bg-primary">
    <ul class="navbar-nav mr-auto">
      <nuxt-link class="navbar-brand" to="/dashboard">CFOS</nuxt-link>  
    </ul>
    <li class="nav-item nav-link text-light">
      Hello, {{ username }}
    </li>
    <li class="nav-item nav-link">
      <button type="button" class="btn btn-danger btn-sm" @click="logOut">Log Out</button>
    </li>
  </nav>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      username: "",
    }
  },
  async created() {
    const config = {
      headers: {
        "Accept" : "application/json"
      }
    };

    let cookie = this.$cookies.get('active-user');

    if(cookie == undefined) {
      this.$router.push("/");
      return;
    }

    try {
      const res = await axios.get(`/api/user/${cookie.id}`, config);
      this.username = res.data.res.username;
    } catch(err) {
      if(err.response) {
        if(err.response.status == 404) {
          this.$router.push("/");
          console.log("THERES NOTHING THERE GORDON");
          return;
        }
      }
    }
  },
  head() {
    return {
      title: "Cafeteria Food Ordering System",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Cafeteria Food Ordering System school project"
        }
      ]
    }
  },
  methods : {
    logOut() {
      this.$cookies.remove('active-user');
      this.$router.push("/");
    }
  }
}
</script>

<style>
html, body {
  height: 100vh;
  margin: 0;
}
</style>