<template>
    <div class="container-fluid">
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
</style>