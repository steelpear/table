<template>
  <v-app>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}&ensp;steelpear@gmail.com</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  head: () => ({
    title: 'Тестовое задание'
  }),
  data: () => ({
    fixed: false
  })
}
</script>
