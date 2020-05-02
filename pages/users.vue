<template>
    <div class="container user-container mt-5 p-3 bg-primary">
        <div class="user-components">
            <UserEdit ref="userEdit" :class="[showUserEdit ? 'd-flex' : 'd-none']" @reload-user="reloadUser" @close-user-edit="closeUserEdit" :id="currentUser"/>
            <h3 class="h1 text-light">Users</h3>
            <User v-for="user in users" v-on:edit-user="editUser" class="border-bottom border-light text-light" :key="user.id" :id="user.id" :username="user.username"/>  
        </div>
        <div class="button-components">
            <button class="btn btn-info" @click="reloadUser()">Refresh</button>
        </div>
    </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import User from '../components/User';
import UserEdit from '../components/UserEdit';

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
        const config = {
            headers: { 'Accept': 'application/json'}
        };

        try {
            const res = await axios.get("http://localhost:5000/user/", config);
            this.users = res.data.res;
        } catch(err) {
            console.log(err);
        }
    },
    methods: {
        async reloadUser() {
            const config = {
                headers: { 'Accept': 'application/json'}
            };

            try {
                this.users = [];
                const res = await axios.get("http://localhost:5000/user/", config);
                console.log(res);
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