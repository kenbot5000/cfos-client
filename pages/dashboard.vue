<template>
    <div class="container main-container mt-5">
        <div class="left-container mr-3">
            <div class="top-container bg-primary mb-3 p-3">
                <h3 class="h2 text-light">Today's Menu</h3>
            </div>
            <div class="bottom-container bg-primary p-3">
                <h3 class="h2 text-light">Users</h3>
                <User v-for="user in firstUsers" class="border-bottom border-light text-light" :key="user.id" :id="user.id" :username="user.username"/>
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

export default {
    components: {
        User,
    },
    data() {
        return {
            users: [],
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