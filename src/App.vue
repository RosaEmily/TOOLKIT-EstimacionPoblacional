<template>
  <b-container>
    <b-card no-body class="mb-2">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <h3 style="float: left!important">Datos</h3>
        <b-button style="float: right!important" v-b-toggle.collapse-1>-</b-button>
      </b-card-header>
      <b-collapse id="collapse-1">
        <b-card-body>
          <b-form>
            <div class="row">
                <div class="col-6">
                  <div class="row">
                    <div class="col-12">
                      <b-form-group
                        id="input-group-1"
                        label="Población"
                        label-for="input-1"
                      >
                        <b-form-input
                          id="input-1"
                          v-model="form.poblacion"
                          type="text"
                          placeholder="Ingrese la población"
                          required
                        ></b-form-input>
                      </b-form-group>
                    </div>
                    <div class="col-6">
                      <b-form-group
                        id="input-group-1"
                        label="Año"
                        label-for="input-1"
                      >
                        <b-form-input
                          id="input-1"
                          v-model="form.año"
                          type="text"
                          placeholder="Ingrese el año"
                          required
                        ></b-form-input>
                      </b-form-group>
                    </div>
                    <div class="col-6">
                      <b-form-group
                        id="input-group-1"
                        label="Año de proyección"
                        label-for="input-1"
                      >
                        <b-form-input
                          id="input-1"
                          v-model="form.proyeccion"
                          type="number"
                          required
                        ></b-form-input>
                      </b-form-group>
                    </div>
                  </div>
                </div>
              <div class="col-6">
                <section class="form">
                  <form action="" class="text-center">
                    <div class="row">
                      <div class="col-5"> <input v-model="año" @keyup.enter="crearCenso" type="text" class="form-control" placeholder="Año"></div>
                      <div class="col-5"> <input v-model="poblacion" @keyup.enter="crearCenso" type="text" name="poblacion" placeholder="Poblacion" class="form-control"></div>
                      <div class="col-2"> <input @click="crearCenso" type="button" value="Añadir" class="btn btn-success"> </div>
                    </div>
                  </form>
              </section>
              <section class="data">
            <caption>Censos</caption>
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">Año</th>
                        <th scope="col">Poblacion</th>
                        <th></th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(Censo, index) in Censos" :key="index">
                        <td>
                            <span v-if="formActualizar && idActualizar == index">
                                <!-- Formulario para actualizar -->
                                <input v-model="añoActualizar" type="text" class="form-control">
                            </span>
                            <span v-else>
                                <!-- Dato año -->
                                {{ Censo.año }}
                            </span>
                        </td>
                        <td>
                            <span v-if="formActualizar && idActualizar == index">
                                <!-- Formulario para actualizar -->
                                <input v-model="poblacionActualizar" type="text" class="form-control">
                            </span>
                            <span v-else>
                                <!-- Dato poblacion -->
                                {{ Censo.poblacion }}
                            </span>
                        </td>
                        <td>
                            <!-- Botón para guardar la información actualizada -->
                            <span v-if="formActualizar && idActualizar == index">
                                <button @click="guardarActualizacion(index)" class="btn btn-success">Guardar</button>
                            </span>
                            <span v-else>
                                <!-- Botón para mostrar el formulario de actualizar -->
                                <button @click="verFormActualizar(index)" class="btn btn-warning">Actualizar</button>
                                <!-- Botón para borrar -->
                                <button @click="borrarCenso(index)" class="btn btn-danger">Borrar</button>
                            </span>
                        </td>
                    </tr>
                </tbody>
            </table>
        </section>
              </div>
            </div>
            <b-button variant="primary" v-on:click="excecuteAlgorithms">EJECUTAR</b-button>
          </b-form>
        </b-card-body>
      </b-collapse>
    </b-card>
    <b-card no-body class="mb-2">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <h3 style="float: left!important">Resultados</h3>
        <b-button style="float: right!important" v-b-toggle.collapse-2>-</b-button>
      </b-card-header>
      <b-collapse id="collapse-2">
        <b-card-body>

        </b-card-body>
      </b-collapse>
    </b-card>
    <b-card no-body class="mb-2">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <h3 style="float: left!important">Proyección anual</h3>
        <b-button style="float: right!important" v-b-toggle.collapse-3>-</b-button>
      </b-card-header>
      <b-collapse id="collapse-3">
        <b-card-body>
          <div v-if="dataSecondAlgorithm.length>0" class="row">
            <div class="col-4">
              <b-table striped hover :items="dataSecondAlgorithm"></b-table>
            </div>
            <div class="col-8">
                <LineChart  :chart-options="chartOptions" :chart-data="chartData"/>
            </div>
          </div>
          <div v-else class="row">
            <b-alert show variant="warning">Para poder visualizar resultados es necesario ejecutar alguno de los métodos de proyección poblacional.</b-alert>
          </div>
        </b-card-body>
      </b-collapse>
    </b-card>
  </b-container>
</template>

<script>
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// Import Bootstrap and BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

// Make BootstrapVue available throughout your project
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)

import { Line as LineChart } from 'vue-chartjs/legacy'

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  CategoryScale,
  PointElement
} from 'chart.js'

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  CategoryScale,
  PointElement
)

export default {
  name: 'App',
  components: {
    LineChart
  },
  data() {
    return {
      // Input año
      año: '',
            // Input poblacion
            poblacion: '',
            // Ver o no ver el formulario de actualizar
            formActualizar: false,
            // La posición de tu lista donde te gustaría actualizar
            idActualizar: -1,
            // Input año dentro del formulario de actualizar
            añoActualizar: '',
            // Input poblacion dentro del formulario de actualizar
            poblacionActualizar: '',
            // Lista de Censos
            Censos: [] ,
      form:[],
      year: 2018,
      foods:[],
      dataAnioAlgorithm: [],
      dataPoblacionFirstAlgorithm: [],
      dataFirstAlgorithm: [],
      dataPoblacionSecondAlgorithm: [],
      dataSecondAlgorithm: [],
      dataPoblacionThirdAlgorithm: [],
      dataThirdAlgorithm: [],
      chartData: {
        labels: [
          'Hola',
          'Hola2',
          'March',
          'April',
          'May',
          'June',
          'July'
        ],
        datasets: [
          {
            label: 'Data One',
            backgroundColor: '#f87979',
            data: [40, 39, 10, 40, 39, 80, 40],
          }
        ]
      },
      items: [
          { Año: 2010, Poblacion: 86.996},
          { Año: 2011, Poblacion: 88.511},
          { Año: 2010, Poblacion: 86.996},
          { Año: 2011, Poblacion: 88.511},
      ],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }

    }
  },
  methods: {
    //this.chartData.datasets[0].data
    calculateYears: function () {
      var year = parseInt(this.form.año);
      var years_proy = parseInt(this.form.proyeccion);
      for(var i=0;i<years_proy;i++){
        this.dataAnioAlgorithm.push(year++);
      }
      /*this.Censos.forEach(()=>{
        year = year +1;
        this.dataAnioAlgorithm.push(year);
      })*/
      console.log("arrayAños",this.dataAnioAlgorithm);
    },
    firstAlgorithm: function () {
      var year = parseInt(this.form.año);
      var population;
      var years_proy = parseInt(this.form.proyeccion);
      for(var i=0;i<years_proy;i++){
        population = parseInt(this.Censos.slice(-1)[0].poblacion)+i+1
        this.dataPoblacionFirstAlgorithm.push(population);
        this.dataFirstAlgorithm.push({Anio:year++,Poblacion:population});
      }
      /*this.Censos.forEach((element)=>{
        year = year +1;
        population = parseInt(element.poblacion)+1
        this.dataPoblacionFirstAlgorithm.push(population);
        this.dataFirstAlgorithm.push({Anio:year+1,Poblacion:population});
      })*/
      console.log("arrayFirst",this.dataFirstAlgorithm);
    },
    secondAlgorithm: function () {
      var year = parseInt(this.form.año);
      var population;
      var years_proy = parseInt(this.form.proyeccion);
      for(var i=0;i<years_proy;i++){
        population = parseInt(this.Censos.slice(-1)[0].poblacion)+i+2
        this.dataPoblacionSecondAlgorithm.push(population);
        this.dataSecondAlgorithm.push({Anio:year++,Poblacion:population});
      }
      /*this.Censos.forEach((element)=>{
        year = year +1;
        population = parseInt(element.poblacion)+2
        this.dataPoblacionSecondAlgorithm.push(population);
        this.dataSecondAlgorithm.push({Anio:year+1,Poblacion:population})
      })*/
      console.log("arraySecond",this.dataSecondAlgorithm);
    },
    thirdAlgorithm: function () {
      var year = parseInt(this.form.año);
      var population;
      var years_proy = parseInt(this.form.proyeccion);
      for(var i=0;i<years_proy;i++){
        population = parseInt(this.Censos.slice(-1)[0].poblacion)+i+3
        this.dataPoblacionThirdAlgorithm.push(population);
        this.dataThirdAlgorithm.push({Anio:year++,Poblacion:population})
      }
      /*this.Censos.forEach((element)=>{
        year = year +1;
        population = parseInt(element.poblacion)+3
        this.dataPoblacionThirdAlgorithm.push(population);
        this.dataThirdAlgorithm.push({Anio:year+1,Poblacion:population})
      })*/
      console.log("arrayThird",this.dataThirdAlgorithm);
    },
    clearDataAlgorithms: function () {
      this.dataAnioAlgorithm = [];
      //this.chartData.datasets[0].data = [40, 39, 10, 40, 39, 80, 40];
      this.dataFirstAlgorithm = [];
      this.dataPoblacionFirstAlgorithm = [];
      this.dataSecondAlgorithm = [];
      this.dataPoblacionSecondAlgorithm = [];
      this.dataThirdAlgorithm = [];
      this.dataPoblacionThirdAlgorithm = [];
    },
    excecuteAlgorithms: function () {
      this.clearDataAlgorithms();
      this.calculateYears();
      this.firstAlgorithm();
      this.secondAlgorithm();
      this.thirdAlgorithm();
      this.chartData.labels = this.dataAnioAlgorithm;
      this.chartData.datasets[0].data = this.dataPoblacionSecondAlgorithm;
    },
    crearCenso: function () {
                // Anyadimos a nuestra lista
                this.Censos.push({
                    id: + new Date(),
                    año: this.año,
                    poblacion: this.poblacion
                });
                // Vaciamos el formulario de añadir
                this.año = '';
                this.poblacion = '';
            },
            verFormActualizar: function (Censo_id) {
                // Antes de mostrar el formulario de actualizar, rellenamos sus campos
                this.idActualizar = Censo_id;
                this.añoActualizar = this.Censos[Censo_id].año;
                this.poblacionActualizar = this.Censos[Censo_id].poblacion;
                // Mostramos el formulario
                this.formActualizar = true;
            },
            borrarCenso: function (Censo_id) {
                // Borramos de la lista
                this.Censos.splice(Censo_id, 1);
            },
            guardarActualizacion: function (Censo_id) {
                // Ocultamos nuestro formulario de actualizar
                this.formActualizar = false;
                // Actualizamos los datos
                this.Censos[Censo_id].año = this.añoActualizar;
                this.Censos[Censo_id].poblacion = this.poblacionActualizar;
            }
  }

}
</script>
