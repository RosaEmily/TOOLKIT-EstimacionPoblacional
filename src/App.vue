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
          <div class="row">
            <div class="col-6">
              <div class="row">
                <div class="col-12">
                  <b-form-group
                    id="input-group-1"
                    label="Mejor Modelo"
                    label-for="input-1"
                  >
                    <b-form-input
                      id="input-1"
                      v-model="bestResult.modelo"
                      type="text" disabled
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
                      v-model="bestResult.año"
                      type="text" disabled
                    ></b-form-input>
                  </b-form-group>
                </div>
                <div class="col-6">
                  <b-form-group
                    id="input-group-1"
                    label="Poblacion"
                    label-for="input-1"
                  >
                    <b-form-input
                      id="input-1"
                      v-model="bestResult.poblacion"
                      type="text" disabled
                    ></b-form-input>
                  </b-form-group>
                </div>
              </div>
            </div>
            <div class="col-6">
              <b-form-group
                    id="input-group-1"
                    label="Resultados Generales"
                    label-for="input-1"
                ></b-form-group>
              <div class="row">
                <b-table striped hover :items="tableResultados"></b-table>
              </div>
            </div>
          </div>
        </b-card-body>
      </b-collapse>
    </b-card>
      <b-card no-body class="mb-2">
        <b-card-header header-tag="header" class="p-1" role="tab">
          <h3 style="float: left!important">Proyección Anual</h3>
          <b-button @click="exportToPDF" :disabled="tableBestAlgorithms.length<=0">EXPORTAR A PDF</b-button>
          <b-button style="float: right!important" v-b-toggle.collapse-3>-</b-button>
        </b-card-header>
        <b-collapse id="collapse-3">
          <b-card-body>
            <b-tabs card>
              <b-tab title="Mejor Proyección" active>
                <div v-if="tableBestAlgorithms.length>0" class="row">
                  <div class="col-4">
                    <b-table-simple>
                      <b-thead class="table-dark">
                        <b-tr>
                          <b-th>Año</b-th>
                          <b-th v-if="tableBestAlgorithms[0].Aritmetico">Aritmético</b-th>
                          <b-th v-if="tableBestAlgorithms[0].Geometrico">Geométrico</b-th>
                          <b-th v-if="tableBestAlgorithms[0].Exponencial">Exponencial</b-th>
                        </b-tr>
                      </b-thead>
                      <b-tbody>
                        <tr v-for="item in tableBestAlgorithms" :key="item.Año" :class="item.variant">
                          <td>{{ item.Año }}</td>
                          <td v-if="item.Aritmetico">{{ item.Aritmetico }}</td>
                          <td v-if="item.Geometrico">{{ item.Geometrico }}</td>
                          <td v-if="item.Exponencial">{{ item.Exponencial }}</td>
                        </tr>
                      </b-tbody>
                    </b-table-simple>
                  </div>
                  <div class="col-8">
                      <LineChart  :chart-options="chartOptions" :chart-data="chartBestData"/>
                  </div>
                </div>
                <div v-else class="row">
                  <b-alert show variant="warning">Para poder visualizar resultados es necesario ejecutar los métodos de proyección poblacional.</b-alert>
                </div>
              </b-tab>
              <b-tab title="Proyecciones">
                <div v-if="tableAlgorithms.length>0" class="row">
                  <div class="col-4">
                    <b-table-simple>
                      <b-thead class="table-dark">
                        <b-tr>
                          <b-th>Año </b-th>
                          <b-th>Aritmético</b-th>
                          <b-th>Geométrico</b-th>
                          <b-th>Exponencial</b-th>
                        </b-tr>
                      </b-thead>
                      <b-tbody>
                        <tr v-for="item in tableAlgorithms" :key="item.Año" :class="item.variant">
                          <td>{{ item.Año }}</td>
                          <td>{{ item.Aritmetico }}</td>
                          <td>{{ item.Geometrico }}</td>
                          <td>{{ item.Exponencial }}</td>
                        </tr>
                      </b-tbody>
                    </b-table-simple>
                  </div>
                  <div class="col-8">
                      <LineChart  :chart-options="chartOptions" :chart-data="chartData"/>
                  </div>
                </div>
                <div v-else class="row">
                  <b-alert show variant="warning">Para poder visualizar resultados es necesario ejecutar los métodos de proyección poblacional.</b-alert>
                </div>
              </b-tab>
            </b-tabs>
          </b-card-body>
        </b-collapse>
        <div>
          <vue-html2pdf
              :show-layout="false"
              :float-layout="true"
              :enable-download="true"
              :preview-modal="true"
              :filename="form.poblacion+'_'+form.año+'_'+form.proyeccion"
              :pdf-quality="2"
              :manual-pagination="true"
              pdf-format="a4"
              pdf-orientation="landscape"
              pdf-content-width="1200px"
              ref="html2Pdf"
          >
              <section slot="pdf-content" style="padding-left: 30px;">
                <div class="row" style="padding-top: 30px;">
                  <h1>TOOLKIT PARA LA ESTIMACIÓN DE POBLACIÓN ANUAL</h1>
                </div>
                <div class="row" style="padding-top: 30px;">
                  <h4>DATOS INGRESADOS</h4>
                  <div style="font-size: 1.2rem;">NOMBRE DE LA POBLACIÓN: {{form.poblacion}}</div>
                  <div style="font-size: 1.2rem;">AÑO BASE: {{form.año}}</div>
                  <div style="font-size: 1.2rem;">AÑOS DE PROYECCIÓN: {{form.proyeccion}}</div>
                </div>
                <div class="row" style="padding-top: 30px;">
                  <h4>RESULTADOS</h4>
                  <div style="font-size: 1.2rem;">MEJOR MODELO: {{bestResult.modelo}}</div>
                  <div style="font-size: 1.2rem;">ERROR DEL MEJOR MODELO: {{bestError}}</div>
                  <div style="font-size: 1.2rem;">ÚLTIMO AÑO DE PROYECCIÓN: {{bestResult.año}}</div>
                  <div style="font-size: 1.2rem;">ÚLTIMA POBLACIÓN DE PROYECCIÓN: {{bestResult.poblacion}}</div>
                </div>
                <div class="html2pdf__page-break"/>
                <div class="row" style="padding-top: 30px;">
                  <h3>TODOS LOS MODELOS - DATOS</h3>
                </div>
                <div v-if="tableAlgorithms.length>0" class="row" style="padding-top: 20px;">
                  <div class="col-12">
                    <b-table-simple>
                      <b-thead class="table-dark">
                        <b-tr>
                          <b-th>Año</b-th>
                          <b-th>Aritmético</b-th>
                          <b-th>Geométrico</b-th>
                          <b-th>Exponencial</b-th>
                        </b-tr>
                      </b-thead>
                      <b-tbody>
                        <tr v-for="(item,index) in tableAlgorithms" :key="index" :class="item.variant">
                          <td>{{ item.Año }}</td>
                          <td>{{ item.Aritmetico }}</td>
                          <td>{{ item.Geometrico }}</td>
                          <td>{{ item.Exponencial }}</td>
                          <div v-if="index&&(index==14||index%18==14)" class="html2pdf__page-break"/>
                          <div v-if="index&&(index==14||index%18==14)" style="height:30px"></div>
                        </tr>
                      </b-tbody>
                    </b-table-simple>
                  </div>
                </div>
                <div class="html2pdf__page-break"/>
                <div class="row" style="padding-top: 30px;">
                  <h3>TODOS LOS MODELOS - GRÁFICO</h3>
                </div>
                <div class="row" style="padding-top: 20px;">
                  <div class="chart-container" style="width:1060px">
                    <LineChart :chart-options="chartOptions" :chart-data="chartData"/>
                  </div>
                </div>
                <div class="html2pdf__page-break"/>
                <div class="row" style="padding-top: 30px;">
                  <h3>MEJOR MODELO - DATOS</h3>
                </div>
                <div v-if="tableBestAlgorithms.length>0" class="row" style="padding-top: 20px;">
                  <div class="col-12">
                    <b-table-simple>
                      <b-thead class="table-dark">
                        <b-tr>
                          <b-th>Año</b-th>
                          <b-th v-if="tableBestAlgorithms[0].Aritmetico">Aritmético</b-th>
                          <b-th v-if="tableBestAlgorithms[0].Geometrico">Geométrico</b-th>
                          <b-th v-if="tableBestAlgorithms[0].Exponencial">Exponencial</b-th>
                        </b-tr>
                      </b-thead>
                      <b-tbody>
                        <tr v-for="(item,index) in tableBestAlgorithms" :key="index" :class="item.variant">
                          <td>{{ item.Año }}</td>
                          <td v-if="item.Aritmetico">{{ item.Aritmetico }}</td>
                          <td v-if="item.Geometrico">{{ item.Geometrico }}</td>
                          <td v-if="item.Exponencial">{{ item.Exponencial }}</td>
                          <div v-if="index&&(index==14||index%18==14)" class="html2pdf__page-break"/>
                          <div v-if="index&&(index==14||index%18==14)" style="height:30px"></div>
                        </tr>
                      </b-tbody>
                    </b-table-simple>
                  </div>
                </div>
                <div class="html2pdf__page-break"/>
                <div class="row" style="padding-top: 30px;">
                  <h3>MEJOR MODELO - GRÁFICO</h3>
                </div>
                <div class="row" style="padding-top: 20px;">
                  <div class="chart-container" style="width:1060px">
                    <LineChart :chart-options="chartOptions" :chart-data="chartBestData"/>
                  </div>
                </div>
              </section>
          </vue-html2pdf>
        </div>
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

import VueHtml2pdf from 'vue-html2pdf'

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
    LineChart,
    VueHtml2pdf
  },
  data() {
    return {
      statusExport: false,
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
      modelAritmetic:{
          incremento:'',
          errorC:'',
      },
      modelGeometric:{
          incremento:'',
          errorC:'',
      },
      modelExponencial:{
          incremento:'',
          errorC:'',
      },
      tableResultados:[],
      bestResult:{
          modelo:'',
          año:'',
          poblacion:'',
      },
      year: 2018,
      foods:[],
      bestModel:0,
      bestError:0,
      tableAlgorithms:[],
      dataAñoAlgorithm: [],
      dataPoblacionArithmeticAlgorithm: [],
      dataArithmeticAlgorithm: [],
      dataPoblacionGeometricAlgorithm: [],
      dataGeometricAlgorithm: [],
      dataPoblacionExponentialAlgorithm: [],
      dataExponentialAlgorithm: [],
      tableBestAlgorithms:[],
      chartBestData:{
        labels: [
          'null',
        ],
        datasets:[
          {
            label: 'best',
            backgroundColor: '#f87979',
            data: [40, 39, 10, 40, 39, 80, 40],
          },
        ]
      },
      chartData: {
        labels: [
          'null'
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
    crearDataCensoPerú(){
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
    crearDataCensoTrujillo(){
      this.Censos=[
        {id:1,año:1940,poblacion:7023111},
        {id:2,año:1961,poblacion:10420357},
        {id:3,año:1972,poblacion:14121564},
        {id:4,año:1981,poblacion:17762231},
        {id:5,año:1993,poblacion:22639443},
        {id:6,año:2007,poblacion:28220764},
        {id:7,año:2017,poblacion:31237385}
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
      this.chartBestData.labels = this.dataAñoAlgorithm;
      console.log("arrayAños",this.dataAñoAlgorithm);
    },

    arithmeticAlgorithm: function () {
      var years_proy = parseInt(this.form.proyeccion);
      var population;
      var i=0
      var k=0
      var n=this.Censos.length
      var r = 0
      for(i=1; i<n;i++){
        k = (this.Censos[i].poblacion - this.Censos[i-1].poblacion)*1.0/(this.Censos[i].año - this.Censos[i-1].año)
        r = r + k;
      }

      r = r/(n-1);
      console.log(r)
      this.modelAritmetic.incremento = r

      for(i=0;i<years_proy+n;i++){
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
      var i=0
      var k=0
      var n=this.Censos.length
      var r = 0
      for(i=1; i<n;i++){
        k = Math.pow((this.Censos[i].poblacion * 1/this.Censos[i-1].poblacion),1/(this.Censos[i].año - this.Censos[i-1].año)) - 1
        r = r + k;
      }

      r = r/(n-1);
      console.log(r)
      this.modelGeometric.incremento = r

      for(i=0;i<years_proy+n;i++){
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
      this.modelExponencial.incremento = r

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
    mejorModelo:function () {
      var suma1 = 0,suma2 = 0,suma3 = 0
      var eca,eca1,eca2,ecg,ecg1,ecg2,ece,ece1,ece2
      var n=this.Censos.length
      var i
      for(i=0; i<n;i++){
        //Aritmetico
        eca1 = 100.0-(this.Censos[i].poblacion*100.0/this.dataPoblacionArithmeticAlgorithm[i]);
        eca2 = Math.abs(eca1/100.0);
        eca = Math.pow(eca2, 2);
        suma1 = suma1 + eca;

        // Modelo geométrico
        ecg1 = 100.0-(this.Censos[i].poblacion*100.0/this.dataPoblacionGeometricAlgorithm[i]);
        ecg2 = Math.abs(ecg1/100.0);
        ecg = Math.pow(ecg2, 2);
        suma2 = suma2 + ecg;

        // Modelo exponencial
        ece1 = 100.0-(this.Censos[i].poblacion*100.0/this.dataPoblacionExponentialAlgorithm[i]);
        ece2 = Math.abs(ece1/100.0);
        ece = Math.pow(ece2, 2);
        suma3 = suma3 + ece;
      }
      var proma = suma1/n;
      var promg = suma2/n;
      var prome = suma3/n;
      this.modelAritmetic.errorC = proma
      this.modelGeometric.errorC = promg
      this.modelExponencial.errorC = prome

      // Selección del mejor modelo
      var mejorModelo = 0;
      var mejorError = 0;   // Falso
      if ( (proma<promg) && (proma<prome) ) {
        mejorModelo = 1;
        mejorError = proma;
      }
      else {
        if ( (promg<proma) && (promg<prome) ) {
          mejorModelo = 2;
          mejorError = promg;
        }
        else {
          if ( (prome<proma) && (prome<promg) ) {
            mejorModelo = 3;
            mejorError = prome;
          }
        }
      }

      this.bestModel= mejorModelo
      this.bestError= mejorError
    },
    clearDataAlgorithms: function () {
      this.dataAñoAlgorithm = [];
      //this.chartData.datasets[0].data = [40, 39, 10, 40, 39, 80, 40];
      this.dataArithmeticAlgorithm = [];
      this.tableAlgorithms = [];
      this.tableBestAlgorithms = [];
      this.dataPoblacionArithmeticAlgorithm = [];
      this.dataGeometricAlgorithm = [];
      this.dataPoblacionGeometricAlgorithm = [];
      this.dataExponentialAlgorithm = [];
      this.dataPoblacionExponentialAlgorithm = [];
      this.bestResult={
          modelo:'',
          año:'',
          poblacion:'',
      };
      this.tableResultados= []
      this.modelAritmetic={
          incremento:'',
          errorC:'',
      }
      this.modelGeometric={
          incremento:'',
          errorC:'',
      }
      this.modelExponencial={
          incremento:'',
          errorC:'',
      }
    },
    clusterAlgorithms: function () {
      var n=this.Censos.length
      var years_proy = parseInt(this.form.proyeccion);
      var i
      for (i = 0; i < years_proy+n; i++) {
        if(i<this.Censos.length) {
          this.tableAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Aritmetico:this.dataArithmeticAlgorithm[i].Poblacion, Geometrico: this.dataGeometricAlgorithm[i].Poblacion, Exponencial: this.dataExponentialAlgorithm[i].Poblacion,variant: 'table-secondary'})
        } else {
          this.tableAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Aritmetico:this.dataArithmeticAlgorithm[i].Poblacion, Geometrico: this.dataGeometricAlgorithm[i].Poblacion, Exponencial: this.dataExponentialAlgorithm[i].Poblacion,variant: ''})
        }
      }

      this.chartData.datasets[0].data = this.dataPoblacionArithmeticAlgorithm;
      this.chartData.datasets[1].data = this.dataPoblacionGeometricAlgorithm;
      this.chartData.datasets[2].data = this.dataPoblacionExponentialAlgorithm;
    },
    selectAlgorithm: function () {
      //select the most accurate algorithm according to chi-square test
      var n=this.Censos.length
      var years_proy = parseInt(this.form.proyeccion);
      var i
      this.mejorModelo()
      if(this.bestModel==1){
        for (i = 0; i < years_proy+n; i++) {
          if(i<this.Censos.length) {
            this.tableBestAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Aritmetico: this.dataArithmeticAlgorithm[i].Poblacion,variant: 'table-secondary'})
          } else {
            this.tableBestAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Aritmetico: this.dataArithmeticAlgorithm[i].Poblacion,variant: ''})
          }
        }
          this.chartBestData.datasets[0].label = 'Aritmetico'
          this.bestResult.modelo= 'Aritmético'
          this.bestResult.poblacion = this.dataPoblacionArithmeticAlgorithm[this.dataPoblacionArithmeticAlgorithm.length-1]
          this.bestResult.año = this.dataAñoAlgorithm[this.dataAñoAlgorithm.length-1]
          this.chartBestData.datasets[0].data = this.dataPoblacionArithmeticAlgorithm;
      }else if(this.bestModel==2){
        for (i = 0; i < years_proy+n; i++) {
          if(i<this.Censos.length) {
            this.tableBestAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Geometrico: this.dataGeometricAlgorithm[i].Poblacion,variant: 'table-secondary'})
          } else {
            this.tableBestAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Geometrico: this.dataGeometricAlgorithm[i].Poblacion,variant: ''})
          }
        }
          this.chartBestData.datasets[0].label = 'Geometrico'
          this.bestResult.modelo= 'Geométrico'
          this.bestResult.poblacion = this.dataPoblacionGeometricAlgorithm[this.dataPoblacionGeometricAlgorithm.length-1]
          this.bestResult.año = this.dataAñoAlgorithm[this.dataAñoAlgorithm.length-1]
          this.chartBestData.datasets[0].data = this.dataPoblacionGeometricAlgorithm;
      } else {
        for (i = 0; i < years_proy+n; i++) {
          if(i<this.Censos.length) {
            this.tableBestAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Exponencial: this.dataExponentialAlgorithm[i].Poblacion,variant: 'table-secondary'})
          } else {
            this.tableBestAlgorithms.push({Año:this.dataArithmeticAlgorithm[i].Año,Exponencial: this.dataExponentialAlgorithm[i].Poblacion,variant: ''})
          }
        }
          this.chartBestData.datasets[0].label = 'Exponencial'
          this.bestResult.modelo = 'Exponencial'
          this.bestResult.poblacion = this.dataPoblacionExponentialAlgorithm[this.dataPoblacionExponentialAlgorithm.length-1]
          this.bestResult.año = this.dataAñoAlgorithm[this.dataAñoAlgorithm.length-1]
          this.chartBestData.datasets[0].data = this.dataPoblacionExponentialAlgorithm;
      }


    },

    excecuteAlgorithms: function () {
      this.clearDataAlgorithms();
      this.calculateYears();
      this.arithmeticAlgorithm();
      this.geometricAlgorithm();
      this.exponentialAlgorithm();
      this.clusterAlgorithms();
      this.selectAlgorithm();
      this.tableResultados.push({Modelo:'Aritmético', Incremento:(this.modelAritmetic.incremento).toFixed(2) , Error: (this.modelAritmetic.errorC).toFixed(6)})
      this.tableResultados.push({Modelo:'Geométrico', Incremento:(this.modelGeometric.incremento).toFixed(7)  , Error: (this.modelGeometric.errorC).toFixed(6)})
      this.tableResultados.push({Modelo:'Exponencial', Incremento:(this.modelExponencial.incremento).toFixed(7)  , Error: (this.modelExponencial.errorC).toFixed(6)})
    },

    exportToPDF: function () {
      /*html2pdf(document.getElementById('table-best'), {
                margin: 10,
              filename: "i-was-html.pdf",
            });*/
      this.$refs.html2Pdf.generatePdf()
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
    this.crearDataCensoTrujillo()
  },


}
</script>