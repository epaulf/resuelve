<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-card class="mx-auto">
          <v-card-title>
            <v-icon large left> mdi-code-json </v-icon>Prueba Ingeniería
            Resuelve:</v-card-title
          >
          <v-file-input
            accept=".txt"
            v-model="file"
            :rules="rulesfile"
            @change="checkfile"
            show-size
            label="Cargar Archivo .txt con json a procesar"
          ></v-file-input>
          <v-card-actions>
            <v-btn :disabled="procesa" @click="procesfile" block color="success"
              >Procesar Archivo</v-btn
            >
          </v-card-actions>
          <vue-json-pretty v-if="jsondata != null" :data="jsondata">
          </vue-json-pretty>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar
      v-model="snackbar"
      :timeout="timeout"
      :color="color"
      :top="'top'"
    >
      {{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn color="white" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
/*
Exercise Resuelve
Author: Eric Paul Flores Espinoza
*/
import VueJsonPretty from "vue-json-pretty";
export default {
  components: {
    VueJsonPretty,
  },
  data: () => ({
    file: [],
    jsondata: null,
    procesa: true,
    rulesfile: [
      (value) =>
        !value ||
        value.size < 2000000 ||
        "Favor de seleccionar un archivo menos a 2 Mb!",
    ],
    // Individual team goals
    niveles: {
      rojo: { A: 5, B: 10, C: 15, CUAUH: 20 },
      azul: { A: 8, B: 10, C: 15, CUAUH: 20 },
      verde: { A: 8, B: 10, C: 15, CUAUH: 20 },
      amarillo: { A: 8, B: 10, C: 15, CUAUH: 20 },
      blanco: { A: 8, B: 10, C: 15, CUAUH: 20 },
      negro: { A: 8, B: 10, C: 15, CUAUH: 20 },
      morado: { A: 8, B: 10, C: 15, CUAUH: 20 },
      naranja: { A: 8, B: 10, C: 15, CUAUH: 20 },
      rosa: { A: 8, B: 10, C: 15, CUAUH: 20 },
    },
    snackbar: false,
    text: "",
    timeout: 5000,
    color: "",
  }),
  methods: {
    // Function to validate that the user uploads a file
    checkfile(event) {
      console.log(event);
      if (event !== void 0 && event.size < 2000000) {
        this.procesa = false;
      } else {
        this.procesa = true;
        this.jsondata = null;
      }
    },
    // Function that obtains the minimum goals according to their level of each player
    getgoles(nivel, colorequipo) {
      return this.niveles[colorequipo][nivel.toUpperCase()];
    },
    // Function that calculates the full salary of each player
    getsueldo(item, colorequipo, golestotales) {
      var metaindividual = this.getporcentaje(
        item.goles,
        this.niveles[colorequipo][item.nivel.toUpperCase()]
      );
      var metagolesequipo = Object.values(this.niveles[colorequipo]).reduce(
        (a, b) => a + b
      );
      var metaequipo = this.getporcentaje(golestotales, metagolesequipo);
      var porcentajebono = metaindividual / 2 + metaequipo / 2;
      return (porcentajebono * item.bono) / 100;
    },
    // Function that obtains the percentage of individual goal
    getporcentaje(goles, golesrequeridos) {
      return (goles * 100) / golesrequeridos;
    },
    // Function that performs the calculations for each player
    procesfile() {
      var reader = new FileReader();
      reader.readAsText(this.file);
      reader.onload = () => {
        try {
          this.jsondata = JSON.parse(reader.result);
          let equipo = null;
          let golestotales = 0;
          let equipos = [...new Set(this.jsondata.map((x) => x.equipo))];
          equipos.forEach(function(itemcolor) {
            equipo = this.jsondata.filter((x) => x.equipo === itemcolor);
            golestotales = equipo.reduce((a, b) => a + b.goles, 0);
            equipo.forEach(function(item) {
              item.goles_minimos = this.getgoles(item.nivel, itemcolor);
              item.sueldo_completo =
                item.sueldo + this.getsueldo(item, itemcolor, golestotales);
            }, this);
          }, this);
        } catch (error) {
          this.snackbar = true;
          this.color = "red";
          this.text = "Archivo txt no contiene Json valido, revise su archivo";
          this.procesa = true;
          this.jsondata = null;
        }
      };
    },
  },
};
</script>
