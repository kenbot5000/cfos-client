<template>
  <div class="container main-container tri-container mt-5">
        <div class="twin-container">
            <div class="left-container bg-secondary mr-3 p-3">
                <h1 class="h1 text-dark">Inactive Orders</h1>
                <table class="table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Price</th>
                            <th>Toggle</th>
                        </tr>
                    </thead>
                    <MenuItem v-for="item in inactiveMenu" :key="item.id" :itemId="item.id" :itemName="item.name" :itemActive="item.active" :itemPrice="item.price"/>
                </table>
            </div>
            <div class="right-container bg-info p-3">
                <h1 class="h1 text-dark">Active Orders</h1>
                <table class="table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Price</th>
                            <th>Toggle</th>
                        </tr>
                    </thead>
                    <MenuItem v-for="item in activeMenu" ref="activeList" @toggle-active="setInactive" @delete-item="activeMenu.splice(item, 1)" :key="item.id" :itemId="item.id" :itemName="item.name" :itemActive="item.active" :itemPrice="item.price"/>
                </table>
            </div>
        </div>
    <div class="toolbar bg-dark mt-4 p-4">
        <button class="btn btn-success">Add New Menu Item</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import MenuItem from '../../components/MenuItem';

export default {
    components: {
        MenuItem,
    },
    data() {
        return {
            menu: []
        }
    },
    computed : {
        activeMenu() {
            return this.menu.filter(function(el) {
                return el.active == true;
            });
        },
        inactiveMenu() {
            return this.menu.filter(function(el) {
                return el.active == false;
            });
        }
    },
    async created() {
        try {
            const menu = await axios.get("/api/menu", config);
            this.menu = menu.data.res;
        } catch(err) {
            console.log(err);
        }
    },
    methods: {
        async setInactive(id) {
            let itemSearch = this.activeMenu.filter(function(el) {
                return el.id == id;
            });
            
            itemSearch[0].active = false;

            console.log(itemSearch);

            // const res = axios.put(`http://localhost:5000/`)

            this.$refs.activeList.splice(id, 1);
        }
    }
}
</script>

<style>
    .tri-container {
        display: flex;
        flex-direction: column;
    }

    .twin-container {
        min-height: 70vh;
        display: flex;
    }

    .toolbar {
        height: 10vh;
        display: flex;
    }
</style>