<template>
  <div style="min-height: 100%; position: relative;">
    <b-navbar toggleable type="dark" variant="dark" class="mb-5" style="padding-left:120px" sticky>
      <b-navbar-brand href="#">PROYECCIÓN POBLACIONAL</b-navbar-brand>
    </b-navbar>
    <b-container style="margin-bottom:100px">
      <b-card no-body class="mb-2">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <h3 style="float: left!important">Datos</h3>
          <b-button style="float: right!important" v-b-toggle.collapse-1>-</b-button>
        </b-card-header>
        <b-collapse visible id="collapse-1">
          <b-card-body>
            <b-form>
              <div class="row">
                  <div class="col-6">
                    <div class="row">
                      <div class="col-12">
                        <b-form-group
                          id="input-group-1"
                          label="Nombre de población"
                          label-for="input-1"
                        >
                          <b-form-input
                            id="input-1"
                            v-model="form.poblacion"
                            type="text"
                            placeholder="Ingrese un nombre para la población"
                          ></b-form-input>
                        </b-form-group>
                      </div>
                      <div class="col-6">
                        <b-form-group
                          id="input-group-1"
                          label="Año base"
                          label-for="input-1"
                        >
                          <b-form-input
                            id="input-1"
                            v-model="form.año"
                            type="text"
                            placeholder="Ingrese el año base"
                          ></b-form-input>
                        </b-form-group>
                      </div>
                      <div class="col-6">
                        <b-form-group
                          id="input-group-1"
                          :label="form.proyeccion?'Años de proyección : ' + form.proyeccion: 'Años de proyección'"
                          label-for="input-1"
                        >
                          <b-form-input id="range-1" v-model="form.proyeccion" type="range" min="1" max="100" class="form-range mt-2"></b-form-input>
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
                                  <b-button v-on:click="guardarActualizacion(index)" class="btn btn-success">Guardar</b-button>
                              </span>
                              <span v-else>
                                  <!-- Botón para mostrar el formulario de actualizar -->
                                  <b-button v-on:click="verFormActualizar(index)" class="btn btn-warning">Actualizar</b-button>
                                  <!-- Botón para borrar -->
                                  <b-button v-on:click="borrarCenso(index)" class="btn btn-danger">Borrar</b-button>
                              </span>
                          </td>
                      </tr>
                  </tbody>
              </table>
          </section>
                </div>
              </div>
              <b-button v-on:click="onSubmit" variant="primary">EJECUTAR</b-button>
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
          <h3 style="float: left!important">Proyección Anual</h3>
          <b-button style="float: right!important" v-b-toggle.collapse-3>-</b-button>
        </b-card-header>
        <b-collapse id="collapse-3">
          <b-card-body>
            <div v-if="tableAlgorithms.length>0" class="row">
              <div class="col-4">
                <b-table striped hover :items="tableAlgorithms"></b-table>
              </div>
              <div class="col-8">
                  <LineChart  :chart-options="chartOptions" :chart-data="chartData"/>
              </div>
            </div>
            <div v-else class="row">
              <b-alert show variant="warning">Para poder visualizar resultados es necesario ejecutar los métodos de proyección poblacional.</b-alert>
            </div>
          </b-card-body>
        </b-collapse>
      </b-card>
    </b-container>
    <footer class="text-center p-3 text-white" style="background-color: rgb(33, 37, 41); position: fixed; bottom: 0; width: 100%;">
      Dr. Santos Fernández Juan Pedro | © 2022 Copyright
    </footer>
  </div>
</template>

<script>
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
import VueSweetalert2 from 'vue-sweetalert2';

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import 'sweetalert2/dist/sweetalert2.min.css';

Vue.use(BootstrapVue)
Vue.use(IconsPlugin)
Vue.use(VueSweetalert2)

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
      tableAlgorithms:[],
      dataAñoAlgorithm: [],
      dataPoblacionArithmeticAlgorithm: [],
      dataArithmeticAlgorithm: [],
      dataPoblacionGeometricAlgorithm: [],
      dataGeometricAlgorithm: [],
      dataPoblacionExponentialAlgorithm: [],
      dataExponentialAlgorithm: [],
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
            label: 'Aritmetico',
            backgroundColor: '#f87979',
            data: [40, 39, 10, 40, 39, 80, 40],
          },
          {
            label: 'Geometrico',
            backgroundColor: '#205DF3',
            data: [40, 39, 10, 40, 39, 80, 40],
          },
          {
            label: 'Exponencial',
            backgroundColor: '#20F33D',
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
    crearDataCenso(){
      this.Censos=[
        {id:1,año:1940,poblacion:36957},
        {id:2,año:1961,poblacion:103320},
        {id:3,año:1972,poblacion:240322},
        {id:4,año:1981,poblacion:354301},
        {id:5,año:1993,poblacion:509312},
        {id:6,año:2007,poblacion:766082},
        {id:7,año:2017,poblacion:919899}
      ]
    },
    //this.chartData.datasets[0].data
    onSubmit: function (event) {
      event.preventDefault();
      if (!this.form.poblacion) {
        this.$swal({
          toast: true,
          icon: 'error',
          title: 'Ingrese un nombre para la población',
          animation: false,
          position: 'bottom-right',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
        });
      } else if (!this.form.año) {
        this.$swal({
          toast: true,
          icon: 'error',
          title: 'Ingrese el año base',
          animation: false,
          position: 'bottom-right',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
        });
      } else if (!this.form.proyeccion) {
        this.$swal({
          toast: true,
          icon: 'error',
          title: 'Ingrese la cantidad de años de proyección',
          animation: false,
          position: 'bottom-right',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
        });
      } else if(this.Censos.length <5) {
        this.$swal({
          toast: true,
          icon: 'error',
          title: 'Para poder realizar la estimación poblacional es necesario contar con una data histórica con longitud mayor o igual a 5',
          animation: false,
          position: 'bottom-right',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
        });
      } else {
        this.excecuteAlgorithms();
      }
    },
    calculateYears: function () {
      //create the array with all years for the estimation
      var year = parseInt(this.form.año);
      var n=this.Censos.length
      var years_proy = parseInt(this.form.proyeccion);
      for(var i=0;i<years_proy+n;i++){
        if(i<n)
          this.dataAñoAlgorithm.push(this.Censos[i].año)
        else
          this.dataAñoAlgorithm.push(year++);
      }
      this.chartData.labels = this.dataAñoAlgorithm;
      console.log("arrayAños",this.dataAñoAlgorithm);
    },

    arithmeticAlgorithm: function () {
      var years_proy = parseInt(this.form.proyeccion);
      var population;

      var n=this.Censos.length
      var r = (this.Censos[n-1].poblacion - this.Censos[0].poblacion)*1.0/(this.Censos[n-1].año - this.Censos[0].año)

      for(var i=0;i<years_proy+n;i++){
        if(i==0){
          population=this.Censos[i].poblacion
          this.dataArithmeticAlgorithm.push({Año:this.Censos[i].año,Poblacion:Math.round(population)});
        }
        else if(i<n){
          population=this.dataPoblacionArithmeticAlgorithm[i-1] + r*(this.Censos[i].año-this.Censos[i-1].año)
          this.dataArithmeticAlgorithm.push({Año:this.Censos[i].año,Poblacion:Math.round(population)});
        }
        else {
          population=this.dataPoblacionArithmeticAlgorithm[i-1] + r*(this.dataAñoAlgorithm[i]-this.dataAñoAlgorithm[i-1])
          this.dataArithmeticAlgorithm.push({Año:this.dataAñoAlgorithm[i],Poblacion: Math.round(population)});
        }
        this.dataPoblacionArithmeticAlgorithm.push(Math.round(population));
      }

      console.log("arrayarithmetic",this.dataArithmeticAlgorithm);
    },
    geometricAlgorithm: function () {
      var years_proy = parseInt(this.form.proyeccion);
      var population;

      var n=this.Censos.length
      var r = Math.pow((this.Censos[n-1].poblacion * 1/this.Censos[0].poblacion),1/(this.Censos[n-1].año - this.Censos[0].año)) - 1

      for(var i=0;i<years_proy+n;i++){
        if(i==0){
          population=this.Censos[i].poblacion
          this.dataGeometricAlgorithm.push({Año:this.Censos[i].año,Poblacion:Math.round(population)});
        }
        else if(i<n){
          population=this.dataPoblacionGeometricAlgorithm[0] * Math.pow((1+r), (this.Censos[i].año-this.Censos[0].año))
          this.dataGeometricAlgorithm.push({Año:this.Censos[i].año,Poblacion:Math.round(population)});
        }
        else {
          population=this.dataPoblacionGeometricAlgorithm[0] * Math.pow((1+r), (this.dataAñoAlgorithm[i]-this.dataAñoAlgorithm[0]))
          this.dataGeometricAlgorithm.push({Año:this.dataAñoAlgorithm[i],Poblacion: Math.round(population)});
        }
        this.dataPoblacionGeometricAlgorithm.push(Math.round(population));
      }
    },
    exponentialAlgorithm: function () {

      var years_proy = parseInt(this.form.proyeccion);
      var population;
      var k=0
      var n=this.Censos.length
      var r = 0
      for(var i=1; i<n;i++){
        k= (Math.log(this.Censos[i].poblacion) - Math.log(this.Censos[i-1].poblacion))/(this.Censos[i].año - this.Censos[i-1].año)
        r = r + k;
      }

      r = r/(n-1);
      console.log(r)

      for(i=0;i<years_proy+n;i++){
        if(i==0){
          population=this.Censos[i].poblacion
          this.dataExponentialAlgorithm.push({Año:this.Censos[i].año,Poblacion:Math.round(population)});
        }
        else if(i<n){
          population=this.dataPoblacionExponentialAlgorithm[0] * Math.exp( r * (this.Censos[i].año-this.Censos[0].año))
          this.dataExponentialAlgorithm.push({Año:this.Censos[i].año,Poblacion:Math.round(population)});
        }
        else {
          population=this.dataPoblacionExponentialAlgorithm[0] * Math.exp( r * (this.dataAñoAlgorithm[i]-this.dataAñoAlgorithm[0]))
          this.dataExponentialAlgorithm.push({Año:this.dataAñoAlgorithm[i],Poblacion: Math.round(population)});
        }
        this.dataPoblacionExponentialAlgorithm.push(Math.round(population));
      }

    },
    clearDataAlgorithms: function () {
      this.dataAñoAlgorithm = [];
      //this.chartData.datasets[0].data = [40, 39, 10, 40, 39, 80, 40];
      this.dataArithmeticAlgorithm = [];
      this.dataPoblacionArithmeticAlgorithm = [];
      this.dataGeometricAlgorithm = [];
      this.dataPoblacionGeometricAlgorithm = [];
      this.dataExponentialAlgorithm = [];
      this.dataPoblacionExponentialAlgorithm = [];
    },
    clusterAlgorithms: function () {
      var n=this.Censos.length
      var years_proy = parseInt(this.form.proyeccion);

      for (var i = 0; i < years_proy+n; i++) {
        this.tableAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Aritmetico:this.dataArithmeticAlgorithm[i].Poblacion, Geometrico: this.dataGeometricAlgorithm[i].Poblacion, Exponencial: this.dataExponentialAlgorithm[i].Poblacion})

      }
    },
    selectAlgorithm: function () {
      //select the most accurate algorithm according to chi-square test
      this.chartData.datasets[0].data = this.dataPoblacionArithmeticAlgorithm;
      this.chartData.datasets[1].data = this.dataPoblacionGeometricAlgorithm;
      this.chartData.datasets[2].data = this.dataPoblacionExponentialAlgorithm;
      this.clusterAlgorithms();
    },

    excecuteAlgorithms: function () {
      this.clearDataAlgorithms();
      this.calculateYears();
      this.arithmeticAlgorithm();
      this.geometricAlgorithm();
      this.exponentialAlgorithm();
      this.selectAlgorithm();
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
  },
  mounted() {
    this.crearDataCenso()
  },


}
</script>
