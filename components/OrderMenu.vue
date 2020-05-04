<template>
    <div class="edit-modal">
        <div class="edit-modal-content m-auto p-4 bg-light">
            <form>
                <div class="form-group">
                    <label>You are Ordering:</label>
                    <input type="text" class="form-control" :value="food.name" readonly disabled>
                </div>
                <div class="form-group">
                    <label>Enter Student Number</label>
                    <input type="text" class="form-control" v-model="student_no">
                    <button type="button" class="btn btn-info d-block mt-2" @click="searchStudent">Search Student</button>
                </div>
                <div class="form-group">
                    <label>Student Name</label>
                    <input type="text" class="form-control" v-model="student_fullname" readonly disabled>
                </div>
                <div class="form-group">
                    <label>Your Orders:</label>
                    <select multiple class="form-control" id="exampleFormControlSelect2" readonly disabled>
                        <option v-for="order in orders" :key=order.id>{{ order.name }}</option>
                    </select>
                </div>
                <span class="text-info d-block mt-2">{{ message }}</span>
                <span class="text-danger d-block mt-2">{{ error }}</span>
                <div class="button-group">
                    <div class="button-group-left">
                        <button type="button" class="btn btn-success" @click="submitOrder">Submit Order</button>
                        <button type="button" class="btn btn-warning" @click="cancelOrder">Cancel Order</button>
                    </div>
                        <button type="button" class="btn btn-danger" @click="preClose">Go Back</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props: ["foodId"],
    data() {
        return {
            food: [],
            student_no: "",
            student: [],
            student_fullname: "",
            orders: [],
            orders_ids: [],
            order_no: null,
            message:"",
            alreadyExistingOrder: true,
            error: ""
        }
    },
    methods: {
        async requestItem(id) {
            try{
                const res = await axios.get(`/api/menu/${id}`);
                this.food = res.data.res;
            } catch(err) {
                console.log(err);
            }
        },
        async searchStudent() {
            try {
                const res = await axios.get(`/api/student/${this.student_no}`);
                this.student = res.data.res;
                this.student_fullname = this.student.lname + ", " + this.student.fname;
                this.error = "";
            } catch(err) {
                // TODO add error message handling
                if(err.response.status == 404) {
                    this.error = "Student does not exist.";
                    return;
                }
            }

            // Search for existing Orders
            try {
                let sn = this.student_no

                const orderRes = await axios.get("/api/order/");
                let existingOrder = orderRes.data.res.filter(function(el) {
                    return el.student_no == sn;
                }).pop();

                if(existingOrder == undefined) {
                    let newOrder = {
                        student_no : sn,
                        orders: [this.food.id]
                    }

                    this.orders = [{ id: 0, name: "You have no existing orders."}];
                    
                    this.alreadyExistingOrder = false;
                } else {
                    const getOrdersFromOrder = await axios.get(`/api/order/${existingOrder.order_no}/items`);
                    this.orders_ids = getOrdersFromOrder.data.orders;
                    this.order_no = existingOrder.order_no;
                    let existingOrderArray = []

                    this.orders_ids.forEach(async function(o) {
                        let getOrderNameFromId = await axios.get(`/api/menu/${o}`);
                        existingOrderArray.push({id: o, name: getOrderNameFromId.data.res.name});
                    });
                        
                    this.orders = existingOrderArray;
                }


            } catch(err) {
                console.log(err);
            }
        },
        async submitOrder() {
            try {
                if(this.alreadyExistingOrder) {
                    let toSubmit = {
                        order_no : this.order_no,
                        order : this.food.id
                    }
                    const res = await axios.put(`/api/order/${this.order_no}`, toSubmit);
                    if(res.status == 201) {
                        this.message = "Order successfully placed! You may now close the form."
                    }
                }
                else {
                    let toSubmit = {
                        student_no: this.student_no,
                        orders: [this.food.id]
                    }

                    const submitNewOrder = await axios.put(`/api/order/`, toSubmit);
                    this.message = "Order successfully placed! You may now close the form."
                }
            } catch(err) {
                console.log(err);
            }
        },
        preClose() {
            this.food = [];
            this.student_no = "";
            this.student = [];
            this.student_fullname = "";
            this.orders = [];
            this.orders_ids = [];
            this.order_no = null;
            this.message = ""
            this.error = ""
            this.$emit('close-form');
        },
        async cancelOrder() {
            try {
                const res = await axios.delete(`/api/order/${this.order_no}`);
                if(res.status == 204) {
                    this.message = "Order cancelled. You may now close the form.";
                }
            } catch(err) {
                console.log(err);
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

.button-group {
    display: flex;
    justify-content: space-between;
}
</style>