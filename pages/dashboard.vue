<template>
    <div class="container main-container mt-5">
        <UserEdit ref="userEdit" :class="[showUserEdit ? 'd-flex' : 'd-none']" @close-user-edit="closeUserEdit" :id="currentUser"/>
        <div class="left-container mr-3">
            <div class="top-container bg-primary mb-3 p-3">
                <h3 class="h2 text-light">Today's Menu</h3>
                <ActiveMenuItem class="border-bottom border-light text-light" v-for="item in activeMenu" :key="item.id" :itemName="item.name" :itemPrice="item.price.toFixed(2)"/>
                <div class="container-fluid text-right">
                    <nuxt-link to="/menu"><button type="button" class="btn btn-outline-info">Manage Menu</button></nuxt-link>
                </div>
            </div>
            <div class="bottom-container bg-primary p-3">
                <h3 class="h2 text-light">Users</h3>
                <User v-for="user in firstUsers" v-on:edit-user="editUser" class="border-bottom border-light text-light" :key="user.id" :userid="user.id" :username="user.username"/>
                <div class="container-fluid text-right">
                    <nuxt-link to="/users"><button type="button" class="btn btn-outline-info mt-2">View All Users</button></nuxt-link>
                </div>
            </div>
        </div>
        <div class="right-container bg-info p-3">
            <h3 class="h2 text-primary">Orders</h3>
                <OrderView class="border-bottom border-dark mt-3" v-for="order in ordersWithNames" :key="order.order_no" :orderno="order.order_no" :studentno="order.student_no" :studentname="order.name" />
        </div>
    </div>
</template>

<script>
import axios from "axios";
import User from '../components/User';
import UserEdit from '../components/UserEdit';
import ActiveMenuItem from '../components/ActiveMenuItem';
import OrderView from '../components/OrderView';

export default {
    components: {
        User,
        UserEdit,
        ActiveMenuItem,
        OrderView
    },
    data() {
        return {
            // Users
            users: [],
            showUserEdit: false,
            currentUser: -1,
            // Active Menu Items
            menu: [],
            orders: [],
            ordersWithNames: []
        }
    },
    computed : {
        firstUsers() {
            return this.users.slice(0, 4);
        },
        activeMenu() {
            return this.menu.filter(function(el) {
                return el.active == true;
            });
        },
    }, 
    async created() {
        // Users
        try {
            const usersRes = await axios.get("/api/user/");
            this.users = usersRes.data.res;
        } catch(err) {
            console.log(err);
        }

        // Menu

        try {
            const menu = await axios.get("/api/menu/");
            this.menu = menu.data.res;
        } catch(err) {
            console.log(err);
        }

        // Orders
        try {
            const orders = await axios.get("/api/order/");
            this.orders = orders.data.res;
            console.log(this.orders);
        } catch(err) {
            console.log(err);
        }

        let bufferArray = [];
        let orders = this.orders;

        for(let i = 0; i < orders.length; i++) {
            let getStudentName = await axios.get(`/api/student/${orders[i].student_no}`);
            let studentName = getStudentName.data.res.lname + ", " + getStudentName.data.res.fname;
            bufferArray.push({order_no: orders[i].order_no, student_no:orders[i].student_no ,name: studentName});
        }

        this.ordersWithNames = bufferArray;
    },
    methods: {
        editUser(id) {  
            this.showUserEdit = true;
            this.currentUser = id;
            this.$refs.userEdit.requestUser(id);
        },
        closeUserEdit() {
            this.showUserEdit = false;
        },
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