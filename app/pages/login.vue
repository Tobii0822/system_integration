<template>
  <div style="text-align: center; margin-top: 50px;">
    <h1>LogInPage</h1>
    <v-card style="max-width: 600px; margin: 50px auto; padding: 20px;">
      <v-text-field
      label="Email"
      type="email"
      outlined
      style="max-width: 600px; margin: 20px auto;">
      </v-text-field>

      <v-text-field
      label="Password"
      type="password"
      outlined
      style="max-width: 600px; margin: 20px auto; border-radius: 50px;">
      </v-text-field>

      <v-btn block variant="tonal" color="primary">Log In</v-btn>

    </v-card>

  </div>
</template>

<script lang="ts" setup>

</script>

<style>

</style>