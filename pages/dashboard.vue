<template>
    <div class="container main-container mt-5">
        <UserEdit ref="userEdit" :class="[showUserEdit ? 'd-flex' : 'd-none']" @close-user-edit="closeUserEdit" :id="currentUser"/>
        <div class="left-container mr-3">
            <div class="top-container bg-primary mb-3 p-3">
                <h3 class="h2 text-light">Today's Menu</h3>
            </div>
            <div class="bottom-container bg-primary p-3">
                <h3 class="h2 text-light">Users</h3>
                <User v-for="user in firstUsers" v-on:edit-user="editUser" class="border-bottom border-light text-light" :key="user.id" :id="user.id" :username="user.username"/>
                <div class="container-fluid text-right">
                    <button type="button" class="btn btn-outline-info mt-2">View All Users</button>
                </div>
            </div>
        </div>
        <div class="right-container bg-info p-3">
            <h3 class="h2 text-primary">Orders</h3>
        </div>
    </div>
</template>

<script>
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
    computed : {
        firstUsers() {
            return this.users.slice(0, 4);
        },
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
    .main-container {
        display: flex;
        min-height: 80vh;
    }

    .left-container {
        display: flex;
        flex-direction: column;
        width: 50%;
    }

    .right-container {
        width: 50%;
    }

    .top-container, .bottom-container {
        height: 40vh;
        overflow: auto;
    }
</style>