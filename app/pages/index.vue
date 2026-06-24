<template>
  
  <div>
    <h1>Hello World</h1>
    <v-btn>Submit</v-btn>

    <v-btn @click="logout">Logout</v-btn>
  </div>


</template>

<script lang="ts" setup>
// @ts-nocheck
  definePageMeta({
    middleware: 'auth'
  })


  const logout = () => {
    localStorage.removeItem('google_user')
    localStorage.removeItem('google_token')
    window.google?.accounts.id.disableAutoSelect()
    navigateTo('/login')
  }
</script>

<style>

</style>




