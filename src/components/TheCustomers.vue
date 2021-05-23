<template>
  <div class="index">

    <v-layout class="mb-5">
      <v-flex class="xs12">
          <h1 class="text-center">Compradores</h1>
      </v-flex>
    </v-layout>

    <v-layout class="d-flex wrap">
      <v-card
      color="deep-purple lighten-5"
        class="mx-auto ma-4"
        width="300"
        v-for="customer in data" :key="customer.id"
      >
        <v-card-text>
          <div>ID: {{ customer.id }}</div>
          <p class="display-1 text--primary">
            {{ customer.name }}
          </p>
          <p>Edad: {{ customer.age }}</p>
          <div class="text--primary">
            Compras realizadas: {{ customer.purchases.length }}
          </div>
        </v-card-text>
      </v-card>
    </v-layout>         
  </div>
</template>

<script>
export default {
  name: 'Customers',
  data(){
    return {
      data: [],
    }
  },
  beforeCreate(){
    if(!localStorage.data) {
      localStorage.setItem("data", JSON.stringify(this.$route.params.data.customers.allCustomers));
    }    
  },
  created() {
    this.data = JSON.parse(localStorage.getItem("data"));
  },
  destroyed() {
    localStorage.removeItem("data");
  },
}
</script>


<style>


</style>