<template>
  <div class="container mt-5 bg-primary p-4">
        <h1 class="h1 text-light">Displaying Order No. {{ $route.params.id }}</h1>
        <form>
            <div class="form-group">
                <label for="studentno" class="text-light">Student No</label>
                <input type="text" name="studentno" class="form-control" v-model="student_no" readonly disabled> 
            </div>
            <div class="form-group">
                <label for="studentname" class="text-light">Student Name</label>
                <input type="text" name="studentname" class="form-control" v-model="student_fullname" readonly disabled> 
            </div>
            <div class="form-group">
                    <label class="text-light">Orders</label>
                    <select multiple class="form-control" readonly disabled>
                        <option v-for="order in orders" :key=order.id>-{{ order.name }}</option>
                    </select>
                </div>
            <div class="buttoncase">
                <div class="buttons-left">
                    <button type="button" @click="cancelOrder" class="btn btn-warning">Complete Order (Delete)</button>
                </div>
                <nuxt-link to="/dashboard"><button type="button" class="btn btn-danger" >Go Back</button></nuxt-link>
            </div>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            student_no: "",
            student: [],
            student_fullname: "",
            orders: [],
            orders_ids: [],
            order_no: null,
        }
    },
    async created() {
        try {
            this.order_no = this.$route.params.id;
            const allOrders = await axios.get(`/api/order/`);
            
            let on = this.order_no;
            let order = allOrders.data.res.filter(function(el) {
                return el.order_no == on;
            }).pop();

            this.student_no = order.student_no;
            const ids = await axios.get(`/api/order/${this.order_no}/items`);
            this.orders_ids = ids.data.orders;

            const studentSearch = await axios.get(`/api/student/${this.student_no}`);
            this.student_fullname = studentSearch.data.res.lname + ", " + studentSearch.data.res.fname;
            
            let existingOrderArray = [];

            this.orders_ids.forEach(async function(o) {
                let getOrderNameFromId = await axios.get(`/api/menu/${o}`);
                existingOrderArray.push({id: o, name: getOrderNameFromId.data.res.name});
            });
                        
            this.orders = existingOrderArray;
        } catch(err) {
           console.log(err);
        }
    },
    methods: {
        async cancelOrder() {
            try {
                const res = await axios.delete(`/api/order/${this.order_no}`);
                if(res.status == 204) {
                    this.$router.push("/dashboard");
                }
            } catch(err) {
                console.log(err);
            }
        }
    }
}
</script>

<style>
.buttoncase {
    display: flex;
    justify-content: space-between;
}
</style>