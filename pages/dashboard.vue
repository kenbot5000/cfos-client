<template>
    <div class="container main-container mt-5">
        <div class="left-container mr-3">
            <div class="top-container bg-dark mb-3 p-3">
                <h3 class="h2 text-light">Today's Menu</h3>
            </div>
            <div class="bottom-container bg-secondary p-3">
                <h3 class="h2 text-dark">Users</h3>
            </div>
        </div>
        <div class="right-container bg-info p-3">
            <h3 class="h2 text-primary">Orders</h3>
        </div>
    </div>
</template>

<script>


export default {
created() {
    
},
}
</script>

<style>
    .main-container {
        display: flex;
        min-height: 80vh;
    }

    .left-container {
        display: flex;
        flex-direction: column;
        width: 50%;
    }

    .right-container {
        width: 50%;
    }

    .top-container, .bottom-container {
        height: 40vh;
    }

    
</style>