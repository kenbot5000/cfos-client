<template>
    <div class="container mt-5 bg-primary p-4">
        <h1 class="h1 text-light">Add A Menu Item</h1>
        <form>
            <div class="form-group">
                <label for="itemname" class="text-light">Item Name</label>
                <input type="text" name="itemname" class="form-control" v-model="newItem.name">
            </div>
            <div class="form-group">
                <label for="itemprice" class="text-light">Price</label>
                <input type="number" step="0.01" name="itemprice" class="form-control" v-model="newItem.price">
            </div>
            <div class="form-check mb-2">    
                <input class="form-check-input" type="checkbox" name="itemactive" v-model="newItem.active">
                <label class="form-check-label text-light" for="itemactive"> Set as Active Menu </label>
            </div>
            <button type="button" class="btn btn-success" @click="submitChanges">Add Item</button>
            <nuxt-link to="/menu"><button type="button" class="btn btn-danger" >Go Back</button></nuxt-link>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            newItem: {
                name: "",
                price: 0.00,
                active: false
            },

        }
    },
    methods : {
        async submitChanges() {
            this.newItem.price = parseFloat(parseFloat(this.newItem.price).toFixed(2));
            console.log(this.newItem.price);
            try {
                const res = await axios.put("/api/menu/", this.newItem);
                this.$router.push("/menu");
            } catch(err) {
                console.log(err);
            }
            
        }
    }
}
</script>

<style>

</style>