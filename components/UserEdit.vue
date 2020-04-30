<template>
    <div class="edit-modal">
        <div class="edit-modal-content m-auto p-4 bg-light">
            <form class="edit-user">
                <div class="form-group">
                    <label for="userid">ID</label>
                    <input type="text" name="userid" class="form-control" :value="id" readonly disabled>
                </div>
                <div class="form-group">
                    <label for="username">Username</label>
                    <input type="text" name="username" class="form-control" v-model="selectedUser.username">
                </div>
                <div class="form-group">
                    <label for="password">Password</label>
                    <input type="text" name="password" class="form-control" v-model="selectedUser.password">
                </div>
                <button type="button" class="btn btn-success" @click="submitChanges">Submit Changes</button>
                <button type="button" class="btn btn-danger" @click="$emit('close-user-edit')">Cancel</button>
                <span class="text-danger d-block" v-if="errorMessage">{{ errorMessage }}</span>
            </form>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "UserEdit",
    props: ["id"],
    data() {
        return {
            selectedUser: [],
            errorMessage: ""
        }
    },
    methods: {
        async requestUser(id) {
            try {
                const res = await axios.get(`http://localhost:5000/user/${id}`);
                this.selectedUser = res.data.res;
            } catch(err) {
                console.log(err);
            }
        },
        async submitChanges() {
            const config = {
                headers: { 'Accept': 'application/json'}
            };

            try {
                let changesToSubmit = {username: this.selectedUser.username, password: this.selectedUser.password};
                console.log(this.selectedUser.id);
                console.log(changesToSubmit);
                const res = await axios.put(`http://localhost:5000/user/${this.selectedUser.id}`, changesToSubmit, config);
            } catch(err) {
                console.log(err);
                this.errorMessage = "Unknown error.";
            }
        }
    }
}
</script>

<style>
.edit-modal {
    display: none;
    flex-direction: column;
    position: fixed; /* Stay in place */
    z-index: 1; /* Sit on top */
    left: 0;
    top: 0;
    width: 100%; /* Full width */
    height: 100%; /* Full height */
    overflow: auto; /* Enable scroll if needed */
    background-color: rgb(0,0,0); /* Fallback color */
    background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

.edit-modal-content {
    width: 50vw;
}


</style>