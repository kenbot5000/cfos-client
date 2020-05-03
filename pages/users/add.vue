<template>
    <div class="container mt-5 p-3 bg-primary">
        <h1 class="h1 text-light">Add A User</h1>
        <form>
            <div class="form-group">
                <label for="username" class="text-light">Username</label>
                <input type="text" name="username" class="form-control" v-model="newUser.username">
            </div>
            <div class="form-group">
                <label for="password" class="text-light">Password</label>
                <input type="text" name="password" class="form-control" v-model="newUser.password">
            </div>
            <span class="text-danger d-block mt-3" v-if="hasError">{{ errorMessage }}</span>
            <button type="button" class="btn btn-success" @click="addUser">Add New User</button>
            <button type="button" class="btn btn-danger" @click="clearForm">Clear</button>
            <nuxt-link to="/users/"> <button type="button" class="btn btn-warning" >Back</button> </nuxt-link>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            newUser : {
                username : "",
                password : "",
            },
            hasError: false,
            errorMessage: ""
        }
    },
    methods: {
        async addUser() {
            const config = {
                headers: {
                    'Accept': 'application/json'
                }
            };

            try {
                const res = await axios.put("http://localhost:5000/user/", this.newUser, config);
                this.$router.push("/users");
            } catch(err) {
                this.errorMessage = err;
                this.hasError = true;
                console.log(err);
            }
        },
        clearForm() {
            this.newUser.username = "";
            this.newUser.password = "";
        }
    }
}
</script>

<style>
</style>