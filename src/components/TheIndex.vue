<template>
    <div style="height:100%">
        <v-layout class="mb-5">
            <v-flex class="xs12">
                <h1 class="text-center">{{ msg }}</h1>
            </v-flex>
        </v-layout> 
        <v-layout class="wrap d-flex align-center pa-2" style="height:50%">
            <v-flex class="xs12 lg4 mt-10 d-flex flex-column justify-center align-center" style="height:100%">
                <div class="mb-13 text-center" style="width:200px">
                    <h2>Cargar datos a la base de datos</h2>
                </div>
                <v-form
                    class="d-flex justify-center flex-column align-center"
                    ref="form"
                    lazy-validation
                >
                    <v-menu
                        v-model="menu"
                        :close-on-content-click="false"
                        transition="scale-transition"
                        class=""
                    >
                        <template v-slot:activator="{ on, attrs }" >
                            <v-text-field
                                v-model="date"
                                label="Consultar fecha"
                                readonly
                                v-bind="attrs"
                                v-on="on"
                                color="deep-purple lighten-1"
                                style="width:200px"
                            ></v-text-field>
                        </template>
                        <v-date-picker
                        v-model="date"
                        @input="menu = false"
                        header-color="deep-purple darken-1"
                        color="deep-purple lighten-1"
                        year-icon="mdi-calendar-blank"
                        ></v-date-picker>
                    </v-menu>
                    <div>
                        <v-btn
                        color="deep-purple darken-2"
                        class="white--text"
                        @click="uploadData($event)"
                        >
                        Cargar datos
                        </v-btn>
                    </div>
                </v-form>
            </v-flex>
            <v-flex class="xs12 lg4 mt-10 d-flex flex-column align-center justify-center" style="height:100%">
                <div class="mb-15 text-center" style="width:200px">
                    <h2>Consultar clientes de la tienda</h2>
                </div>
                <div class="mt-16">
                    <v-btn
                    color="deep-purple darken-2"
                    class="white--text"
                    @click="getCustomers($event)"
                    >
                    Clientes
                    </v-btn>
                </div>
            </v-flex>
            <v-flex class="xs12 lg4 mt-10 d-flex flex-column align-center justify-center" style="height:100%">
                <div class="mb-13 text-center" style="width:200px">
                    <h2>Reporte de un cliente</h2>
                </div>
                <v-form
                    class="d-flex justify-center flex-column align-center"
                    ref="form"
                    lazy-validation
                > 
                    <div>
                        <v-text-field
                            v-model="id"
                            label="Escribe el id"
                            color="deep-purple lighten-1"
                            style="width:200px"
                        ></v-text-field>
                    </div>
                    <div>
                        <v-btn
                        color="deep-purple darken-2"
                        class="white--text"
                        @click="getCustomer($event)"
                        >
                        Consultar cliente
                        </v-btn>
                    </div>
                </v-form>
            </v-flex>
        </v-layout>
    </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
  name: 'Index',
  props: {
    msg: String
  },
  data() {
      return {
          info: {},
          date: new Date().toISOString().substr(0, 10),
          menu: false,
          id: "",
      }
      
  },
  methods: {
      async uploadData(event) {
          if(event) {
              event.preventDefault();

              //YYYY-MM-DD
              let dateArray = this.date.split("-");
              let dateUnix = new Date(parseInt(dateArray[0], 10), parseInt(dateArray[1], 10) - 1, parseInt(dateArray[2], 10)).getTime()/1000;

              try {
                  let response = await fetch('http://localhost:3500/data', {
                      method: 'post',
                      headers: {
                          'Content-Type': 'application/json'
                      },
                      body: JSON.stringify({Date: dateUnix.toString()})
                  });
                  response = await response.json();

                  if (response.status) {
                      Swal.fire({
                          icon: 'success',
                          title: 'Súper!',
                          text: `${response.message}`
                      });

                  } else {
                      Swal.fire({
                          icon: 'error',
                          title: 'Oops...',
                          text: `${response.message}`
                      });
                  }
              } catch (error) {
                  Swal.fire({
                      icon: 'error',
                      title: 'Oops...',
                      text: 'Ha ocurrido un error al cargar los datos'
                  });
              }
          }
      },
      async getCustomers(event) {
          if (event) {
              event.preventDefault();

              try {
                  let response = await fetch('http://localhost:3500/customers', {
                      method: 'get',
                  });
                  response = await response.json();

                  if (response.status) {
                      Swal.fire({
                          icon: 'success',
                          title: 'Súper!',
                          text: `${response.message}`
                      });

                      this.$router.push({
                          name: "Customers",
                          params: {data: response}
                      });
                  } else {
                      Swal.fire({
                          icon: 'error',
                          title: 'Oops...',
                          text: `${response.message}`
                      });
                  }                    
              } catch (error) {
                  Swal.fire({
                      icon: 'error',
                      title: 'Oops...',
                      text: 'Ha ocurrido un error'
                  });
              }
          }
      },

      async getCustomer(event) {
          if (this.id !== "") {
              event.preventDefault();

              try {
                  let response = await fetch(`http://localhost:3500/customer/${this.id}`, {
                      method: 'get',
                  });
                  response = await response.json();
                  if (response.status) {
                      
                      Swal.fire({
                          icon: 'success',
                          title: 'Súper!',
                          text: `${response.message}`
                      });
    
                      this.$router.push({
                          path: `/customer/${this.id}`,
                          query: {data: response}
                      });
                  } else {
                      Swal.fire({
                          icon: 'error',
                          title: 'Oops...',
                          text: `${response.message}`
                      });
                  }                    
              } catch (error) {
                  Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: 'Ha ocurrido un error'
                  });
              }
          } else {
              Swal.fire({
                icon: 'error',
                title: 'Oops...',
                text: 'No has introducido un id válido'
              });
          }
      },

  }
}
</script>


<style>

h1 {
    font-size: 50px;
}


</style>