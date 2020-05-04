<template>
    <div class="container">
        <OrderMenu ref="orderForm" @close-form="closeOrder" :class="[showOrderForm ? 'd-flex' : 'd-none']" :foodId="currentItem"/>
        <ClientMenu v-for="item in menu" :key="item.id" @order="openOrder" :foodId="item.id" :foodName="item.name" :foodPrice="item.price.toFixed(2)"/>
    </div>
</template>

<script>
import ClientMenu from '../components/ClientMenu';
import OrderMenu from '../components/OrderMenu';
import axios from 'axios';

export default {
    layout: "clientLayout",
    components: {
        ClientMenu,
        OrderMenu
    },
    data() {
        return {
            menu: [],
            showOrderForm: false,
            currentItem: -1,
        }
    },
    async created() {
        try {
            const res = await axios.get("/api/menu/");
            this.menu = res.data.res.filter(function(el) {
                return el.active == true;
            }).sort(function(a, b) {
                return a.id - b.id
            });
        } catch(err) {
            console.log(err);
        }
    },
    methods: {
        openOrder(id) {
            this.showOrderForm = true;
            this.$refs.orderForm.requestItem(id);
        },
        closeOrder() {
            this.showOrderForm = false;
        }
    }
}
</script>

<style>

</style>