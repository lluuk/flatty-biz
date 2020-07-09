<template>
  <v-app id="inspire">
    <v-app-bar app clipped-left color="primary">
      <v-toolbar-title class="mr-12 align-center">
        <span class="title">Flatty Biz</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        v-if="$route.name === 'signup' || $route.name === 'login'"
        text
        :to="$route.name === 'signup' ? '/login' : '/signup'"
        >{{ $route.name === 'signup' ? 'Login' : 'Signup' }}</v-btn
      >
    </v-app-bar>
    <v-main>
      <v-container class="fill-height" fluid>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  middleware: ['guest'],
}
</script>
