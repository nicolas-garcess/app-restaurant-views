<template>
  <div class="index">

    <v-layout class="mb-5">
      <v-flex class="xs12">
          <h1 class="text-center">Información del cliente</h1>
      </v-flex>
    </v-layout>

    <div>
      <h2 class="display-1 mb-5 xs12 text-center">Historial de compras: {{ data.customer.customer[0].name }}</h2>
    </div>

    <v-layout class="d-flex wrap justify-center">
      <v-card
      color="deep-purple lighten-5"
        class="ma-2"
        width="330"
        v-for="purchase in data.customer.customer[0].purchases " :key="purchase.id"
      >
        <v-card-text>

          <div>IP: {{ purchase.ip }}</div>
          <p class="title text--primary">
            ID compra: {{ purchase.idPurchase }}
          </p>
          <p>Dispositivo: {{ purchase.device.toUpperCase() }}</p>
          <p class="text-center headline">Productos</p>
          <v-layout class="d-flex flex-column">

            <v-flex class="d-flex mb-3">
              <v-flex class="xs4 text-center">ID</v-flex>
              <v-flex class="xs4 text-center">Nombre</v-flex>
              <v-flex class="xs4 text-center">Precio</v-flex>
            </v-flex>

            <v-flex class="d-flex align-center" v-for="product in purchase.products" :key="product.id">
              <v-flex class="mb-2 xs4 text-center">{{ product.idProduct }}</v-flex>
              <v-flex v-if="product.productName" class="mb-2 xs4 text-center">{{ product.productName }}</v-flex>
              <v-flex v-if="product.price" class="mb-2 xs4 text-center">{{ product.price }}</v-flex>
            </v-flex>

          </v-layout>
        </v-card-text>
      </v-card>
    </v-layout>

    <div class="mt-10">
      <h2 class="display-1 text-center">Clientes con la misma IP</h2>
    </div>

    <v-layout class="d-flex wrap mt-5">
      <v-card
      color="deep-purple lighten-5"
        class="ma-4 text-center"
        width="330"
        v-for="info in data.customersSameIP " :key="info.id"
      >
        <v-card-text>
          <div>IP: {{ info.customers[0].ip }}</div>
          <p class="headline mt-3">Nombre</p>
          <p class="title text--primary" v-for="client in info.customers[0][variable]" :key="client.id">
            {{ client.name }}
          </p>
        </v-card-text>
      </v-card>
    </v-layout>

    <div class="mt-10">
      <h2 class="display-1 text-center">Productos recomendados</h2>
    </div>

    <v-layout class="d-flex wrap mt-5">
      <v-card
      color="deep-purple lighten-5"
        class="ma-4 text-center"
        width="330"
        v-for="product in data.mostSoldProducts.mostSoldProducts" :key="product.id"        
      >
        <v-card-text>
          <div>ID: {{ product.idProduct }}</div>
        
          <p class="title text--primary">
            Producto: {{ product.productName }}
          </p>
          <p class="">
            Ventas totales: {{ product.total }}
          </p>
        </v-card-text>
      </v-card>
    </v-layout>
    
  </div>
</template>

<script>
export default {
  name: 'Customer',
  //props: ['item'],
  data(){
    return {
      data: [],
      variable: "",
    }
  },
  beforeCreate(){
    if(!localStorage.data) {
      localStorage.setItem("data", JSON.stringify(this.$route.query.data));
    }    
  },
  created() {
    //this.data = this.$route.query.data
    this.variable = "~purchases"
    this.data = JSON.parse(localStorage.getItem("data"));
  },
  destroyed() {
    localStorage.removeItem("data");
  },
}
</script>


<style>
.index {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items:center;
}
.button, .send {
    width: 240px;
    height: 60px;
    margin: 20px;
    padding: 5px;
    border-radius: 10px;
}

.button__link, .send {
    font-size: 25px;
}

</style>