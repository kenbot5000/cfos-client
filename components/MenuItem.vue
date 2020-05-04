<template>
    <!-- <div class="menu-item mb-2 py-2">
        <span class="mt-2"><span class="h5">{{itemName}}</span> - {{itemPrice}}</span>
        <button class="btn btn-primary">{{ buttonMessage }}</button>
        
    </div> -->
    <tr>
        <td class="align-middle">{{ itemId }}</td>
        <td class="align-middle name-cell">{{ itemName }}</td>
        <td class="align-middle">{{ itemPrice }}</td>
        <td><button class="btn btn-sm btn-primary" @click="$emit('toggle-active', itemId)">{{ buttonMessage }}</button></td>
    </tr>
</template>

<script>
export default {
    props: ["itemName", "itemPrice", 'itemActive', 'itemId'],
    computed: {
        buttonMessage() {
            if(this.itemActive == true) {
                return "Set as Inactive"
            } else {
                return "Set as Active"
            }
        }
    },
    methods: {
        deleteSelf() {
            this.$emit("delete-item");
        }
    }
}
</script>

<style>
.menu-item {
    display: flex;
    justify-content: space-between;
}

.name-cell {
    max-width: 15vw;
}   
</style>