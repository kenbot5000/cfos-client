<template>
    <div class="container-fluid bg-light">
        <AppHeader/>
        <nuxt/>
    </div>
</template>

<script>
import AppHeader from '../components/AppHeader';

export default {
    components: {
        AppHeader
    }
}
</script>

<style>
  .container-fluid {
    padding: 0;
  }

  body {
    background-color: #F2F2F0 !important; 
  }
</style>