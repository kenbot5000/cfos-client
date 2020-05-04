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
                    <MenuItem v-for="item in inactiveMenu" ref="inactiveList" @toggle-active="setActive" :key="item.id" :itemId="item.id" :itemName="item.name" :itemActive="item.active" :itemPrice="item.price.toFixed(2)"/>
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
                    <MenuItem v-for="item in activeMenu" ref="activeList" @toggle-active="setInactive" @delete-item="activeMenu.splice(item, 1)" :key="item.id" :itemId="item.id" :itemName="item.name" :itemActive="item.active" :itemPrice="item.price.toFixed(2)"/>
                </table>
            </div>
        </div>
    <div class="toolbar bg-dark mt-4 p-4">
        <div class="toolbar-buttons">
            <button class="btn btn-success mr-2" @click="routeTo(0)">Add New Menu Item</button>
            <button class="btn btn-info mr-2">Refresh</button>
            <button class="btn btn-warning mr-2" @click="routeTo(1)">Edit an Item</button> 
        </div>
            <button class="btn btn-danger" @click="routeTo(2)">Go Back</button>
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
            }).sort(function(a, b) {
                return a.id - b.id
            });
        },
        inactiveMenu() {
            return this.menu.filter(function(el) {
                return el.active == false;
            }).sort(function(a, b) {
                return a.id - b.id
            });
        }
    },
    async created() {
        try {
            const menu = await axios.get("/api/menu/");
            this.menu = menu.data.res;
        } catch(err) {
            console.log(err);
        }
    },
    methods: {
        routeTo(num) {
            let routes = ["/menu/add", "/menu/edit", "/dashboard"];
            this.$router.push(routes[num]);
        },
        async setInactive(id) {
            let itemSearch = this.activeMenu.filter(function(el) {
                return el.id == id;
            });
            
            itemSearch[0].active = false;

            try {
                const res = axios.put(`/api/menu/${itemSearch[0].id}`, itemSearch[0]);
            } catch(err) {
                console.log(err);
            }

            this.$refs.activeList.splice(id, 1);
        },
        async setActive(id) {
            let itemSearch = this.inactiveMenu.filter(function(el) {
                return el.id == id;
            });

            itemSearch[0].active = true;
            
            try {
                const res = axios.put(`/api/menu/${itemSearch[0].id}`, itemSearch[0]);
            } catch(err) {
                console.log(err);
            }
            
            this.$refs.inactiveList.splice(id, 1);
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
        max-height: 75vh;
        display: flex;
    }

    .toolbar {
        height: 10vh;
        display: flex;
        justify-content: space-between;
    }

    .toolbar-buttons {
        display: flex;
        align-items: middle;
    }

    .twin-container > .left-container, .twin-container > .right-container {
        overflow: auto;
    }
</style>