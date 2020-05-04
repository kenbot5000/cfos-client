<template>
  <div class="container-fluid kiosk">
      <div class="container p-0">
        <nav class="navbar-expand-lg navbar-fixed pl-5">
            <ul class="navbar-nav mr-auto">
                <h1 class="h1 text-light">Today's Menu</h1>
            </ul>
        </nav>
        <nuxt/>
      </div>
  </div>
</template>

<script>
export default {

}
</script>

<style>
.kiosk {
    height: 100vh;
    background-color: #fccab5;
}

.navbar-fixed {
    box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.25);
    width: 100%;
    background-color: #F6B092;
}

.kiosk > .container {
    background-color: #F6A192;
    height: 100%;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.25);
}


</style>