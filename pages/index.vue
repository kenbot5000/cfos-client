<template>
  <div id="main" class="text-center bg-primary">
    <div id="login-form" class="m-auto">
      <h2 class="h2 mb-4 text-light">Login to CFOS</h2>
      <form class="form-signin" @submit.prevent="login">
        <input type="text" id="txt-username" name="username" class="form-control" placeholder="Username" v-model="user.username">
        <input type="password" id="txt-password" name="password" class="form-control" placeholder="Password" v-model="user.password">
        <span v-if="hasError" class="d-block text-danger mt-3">{{ error }}</span>
        <button type="submit" class="btn btn-outline-light mt-3">Log In</button>  
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import noLayoutVue from '../layouts/noLayout.vue';

export default {
  layout: 'noLayout',
  data() {
    return {
      // TODO: Make into a hash verification
      user: {
        username: '',
        password: ''
      },
      error: '',
      hasError: false
    }
  },
  methods: {
    async login() {
      try {
        let text = this.user.username;
        const res = await axios.get(`/api/user/${text}`);
        if(this.user.password == res.data.res.password) {
          // var d = new Date();
          // d.setTime(d.getTime() + (4*60*60*1000));
          // var expires = d.toUTCString();
          // document.cookie = `id=${res.data.res.id}; expires=${expires};path=/`;
          const cookie = {'id' : res.data.res.id};
          this.$cookies.set('active-user', cookie, {
            path: '/',
            maxAge: 60 * 60 * 16
          });
          
          this.$router.push("dashboard");
        }
        else {
          this.error = "Incorrect password."
          this.hasError = true;
        }
      } catch(err) {
        if(err.response.status == 404) {
          this.error = "User does not exist.";
          this.hasError = true;
        } else {
          this.error = "Unknown error";
          this.hasError = true;
        }
        
     }
    }
  }
}
</script>

<style>
#main {
  height: 100vh;
  display: flex;
}

#login-form {
  max-width: 330px;
  width: 100%;
  display: block;
  flex-direction: column;
}

#txt-username {
  border-bottom-left-radius: 0px;
  border-bottom-right-radius: 0px;
}

#txt-password {
  border-top-left-radius: 0px;
  border-top-right-radius: 0px;
}
</style>