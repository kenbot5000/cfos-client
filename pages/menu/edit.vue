<template>
    <div class="container mt-5 bg-primary p-4">
        <h1 class="h1 text-light">Add A Menu Item</h1>
        <form>
            <div class="form-group">
                <label for="itemname" class="text-light">Item ID</label>
                <input type="text" name="itemname" class="form-control" v-model="idSearch"> 
                <span class="text-danger d-block mt-2" v-if="hasError">{{ error }}</span>
                <button type="button" @click="searchId" class="btn btn-secondary mt-2"> Search for ID </button>
            </div>
            <div class="form-group">
                <label for="itemname" class="text-light">Item Name</label>
                <input type="text" name="itemname" class="form-control" v-model="editItem.name">
            </div>
            <div class="form-group">
                <label for="itemprice" class="text-light">Price</label>
                <input type="number" step="0.01" name="itemprice" class="form-control" v-model="editItem.price">
            </div>
            <div class="form-check mb-2">    
                <input class="form-check-input" type="checkbox" name="itemactive" v-model="editItem.active">
                <label class="form-check-label text-light" for="itemactive"> Set as Active Menu </label>
            </div>
            <div class="buttoncase">
                <div class="buttons-left">
                    <button type="button" class="btn btn-success" @click="submitChanges">Save Changes</button>
                    <button type="button" class="btn btn-warning">Delete</button>
                </div>
                <nuxt-link to="/menu"><button type="button" class="btn btn-danger" >Go Back</button></nuxt-link>
            </div>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            idSearch: "",
            editItem: [],
            error: "",
            hasError: false
        }
    },
    methods : {
        async searchId() {
            try {
                const item = await axios.get(`/api/menu/${this.idSearch}`);
                
                this.editItem = item.data.res;

                this.error = "";
                this.hasError = false;
            } catch(err) {
                if(err.response.status == 404) {
                    this.error = "Item does not exist.";
                    this.hasError = true;
                } else {
                    this.error = "Unknown error";
                    this.hasError = true;
                }
            }
        },
        async submitChanges() {
            this.editItem.price = parseFloat(parseFloat(this.editItem.price).toFixed(2));
            
            let toSubmit = {
                name: this.editItem.name,
                price: this.editItem.price,
                active: this.editItem.active
            };

            if(this.hasError) {
                this.error = "Form reset needed. Search ID again."
            }

            try {
                const res = await axios.put(`/api/menu/${this.idSearch}`, toSubmit);
                if(res.status == 204) {
                    this.$router.push("/menu");
                }
            } catch(err) {
                console.log(err)
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