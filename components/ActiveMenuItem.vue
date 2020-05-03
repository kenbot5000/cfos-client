<template>
    <div class="menu mb-2 py-2">
        <span class="h5 mt-2">{{itemName}} - {{itemPrice}}</span>
    </div>
</template>

<script>
export default {
    props: ["itemName", "itemPrice"]
}
</script>

<style>
.menu {
    display: flex;
    justify-content: space-between;
}
</style>