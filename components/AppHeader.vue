<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <ul class="navbar-nav mr-auto">
      <nuxt-link class="navbar-brand" to="/dashboard">CFOS</nuxt-link>
      <li class="nav-item dropdown text-light">
        <a class="nav-link dropdown-toggle text-light" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Hello, {{ username }}
        </a>
        <div class="dropdown-menu" aria-labelledby="navbarDropdown">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <div class="dropdown-divider"></div>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </li>
    </ul>
    
  </nav>
</template>

<script>
import axios from "axios";
import 'jquery';

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
      const res = await axios.get(`http://localhost:5000/user/${cookie.id}`, config);
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
  }
}
</script>

<style>

</style>