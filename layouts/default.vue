<template>
  <v-app dark>
    <v-content>
      <Menu/>
      <nuxt />
    </v-content>
  </v-app>
</template>

<script>

  import Menu from "../components/Menu";
  export default {
    components: {Menu},
    data () {
      return {}
    },
  }
</script>
