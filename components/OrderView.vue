<template>
  <div class="ordercon d-flex mb-2 py-2">
    <span class="h5 mt-2">{{studentno}} - Order No. {{ orderno }}</span>
    <button class="btn btn-secondary text-dark" @click="routeToId">View</button>
  </div>
</template>

<script>
export default {
    props: ["studentno", "orderno", "studentname"],
    data() {
        return {
            order_no: this.orderno
        }
    },
    methods : {
        routeToId() {
            this.$router.push(`/orders/${this.order_no}`);
        }
    }

}
</script>

<style>
.ordercon {
    justify-content: space-between;
}
</style>