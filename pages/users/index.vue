<template>
    <div class="container user-container mt-5 p-3 bg-primary">
        <div class="user-components">
            <UserEdit ref="userEdit" :class="[showUserEdit ? 'd-flex' : 'd-none']" @reload-user="reloadUser" @close-user-edit="closeUserEdit" :id="currentUser"/>
            <h1 class="h1 text-light">Users</h1>
            <User v-for="(user, index) in users" @edit-user="editUser" @delete-user="users.splice(index, 1)" class="border-bottom border-light text-light" :key="user.id" :userid="user.id" :username="user.username"/>  
        </div>
        <div class="button-components">
            <button class="btn btn-info" @click="reloadUser()">Refresh</button>
            <nuxt-link to="/users/add"><button class="btn btn-success">Add New User</button></nuxt-link>
            <nuxt-link to="/dashboard"><button class="btn btn-danger">Go Back</button></nuxt-link>
        </div>
    </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import User from '../../components/User';
import UserEdit from '../../components/UserEdit';

export default {
    components: {
        User,
        UserEdit,
    },
    data() {
        return {
            users: [],
            showUserEdit: false,
            currentUser: -1,
        }
    },
    async created() {
        try {
            const res = await axios.get("/api/user/");
            this.users = res.data.res;
        } catch(err) {
            console.log(err);
        }
    },
    methods: {
        async reloadUser() {
            try {
                this.users = [];
                const res = await axios.get("/api/user/");
                this.users = res.data.res;
            } catch(err) {
                console.log(err);
             }
        },
        editUser(id) {  
            this.showUserEdit = true;
            this.currentUser = id;
            this.$refs.userEdit.requestUser(id);
        },
        closeUserEdit() {
            this.showUserEdit = false;
        }
    }
}
</script>

<style>
.user-container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 80vh;
}

.button-components {
    align-self: flex-end;
}
</style>