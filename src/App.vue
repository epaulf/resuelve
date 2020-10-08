<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <span class="mr-2">Ejercicio Resuelve</span>
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://epaul.dev/" target="_blank" text>
        <span class="mr-2">Eric Paul Flores Espinoza</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
