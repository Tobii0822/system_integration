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

  const user = ref<any>(null)
  const showScanner = ref(false)
  const scannedValue = ref ('')

  onMounted(() => {
    const savedUser = localStorage.getItem('google_user')

    if (!savedUser) {
      navigateTo('/login')
      return
    }

    user.value = JSON.parse(savedUser)
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




