<template>
    <div class="menu mb-2 py-2">
        <span class="mt-2"><span class="h5">{{itemName}}</span> - {{itemPrice}}</span>
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