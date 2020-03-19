<template>
  <v-app dark>
    <v-content>
      <v-navigation-drawer
        app
        fill-height
        floating
        dark
      >
        <v-list
          dense
          nav
          class="py-0"
        >
          <v-list-item two-line class="px-0">
            <v-list-item-content>
              <v-list-item-title>Chuuuuut</v-list-item-title>
              <v-list-item-subtitle>Accès pour Ynov</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          <v-divider v-if="!isLogged"></v-divider>
          <v-list-item v-if="!isLogged" class="pa-0">
            <v-list-item-content>
              <v-row>
                <v-col cols="12">
                  <v-text-field v-model="email" label="Email" outlined dense hide-details/>
                </v-col>
                <v-col cols="12">
                  <v-text-field label="Password" v-model="password" outlined dense hide-details/>
                </v-col>
                <v-col cols="6" >
                  <v-btn small outlined>Inscription</v-btn>
                </v-col>
                <v-col cols="6" >
                  <v-btn small outlined>connexion</v-btn>
                </v-col>
              </v-row>
            </v-list-item-content>
          </v-list-item>
          <v-divider></v-divider>

        </v-list>
      </v-navigation-drawer>
      <nuxt />
    </v-content>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        email: '',
        password: '',
        isLogged: false,
        logInToggle: false,
      }
    },
  }
</script>
