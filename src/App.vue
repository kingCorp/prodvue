<template>
  <div id="app">
    <div class="container p-3">
      <h3>Products</h3>
      <hr>
      <h3>Create product</h3>
      <hr>
      <form @submit.prevent="saveProduct" class="mb-3">
      <div class="form-group">
        <input type="text" class="form-control" placeholder="Name" v-model="name">
      </div>
      <div class="form-group">
        <input type="text" class="form-control" placeholder="Category" v-model="category">
      </div>
      <div class="form-group">
        <input type="number" class="form-control" placeholder="Price" v-model="price">
      </div>
      <div class="form-group">
        <input type="text" class="form-control" placeholder="Color" v-model="color">
      </div>
      <div class="form-group">
        <input type="text" class="form-control" placeholder="Description" v-model="description">
      </div>
      <button class="btn btn-success btn-block">Save</button>
    </form>
      <hr>
      <h4>list of available products</h4>
      <div class="container row">
        <div class="col-md-6">
          <div class="card card-body mb-2" v-for="product in products" v-bind:key="product._id">
            <h3>{{ product.name }}</h3>
            <p>${{ product.price}}</p>
            <button @click="getSingleProduct(product._id)" class="btn btn-success mb-2">View</button>
          </div>
        </div>
        <div class="col-md-6">
          <div>
            <img src='https://media.wired.com/photos/5b22c5c4b878a15e9ce80d92/master/pass/iphonex-TA.jpg' class="img-thumbnail" >
            <h5>{{prod.name}}</h5>
            <p>{{prod.description}}</p>
            <p>{{prod.category}}</p>
            <p style="font-size:30px">${{prod.price}}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from "axios";
export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      products: [],
      name: "",
      price: "",
      category: "",
      color: "",
      description: "",
      image: 'https://media.wired.com/photos/5b22c5c4b878a15e9ce80d92/master/pass/iphonex-TA.jpg',
      prod: {}
    };
  },
  methods: {
    getProducts() {
      axios
        .get("https://pdapi.herokuapp.com/v1/products")
        .then(res => {
          console.log(res.data);
          this.products = res.data.product;
        })
        .catch(err => console.log(err));
    },
    getSingleProduct(id) {
      axios
        .get("https://pdapi.herokuapp.com/v1/products/" + id)
        .then(res => {
          console.log(res.data);
          this.prod = res.data.doc;
        })
        .catch(err => console.log(err));
    },
    saveProduct() {
      axios
        .post("https://pdapi.herokuapp.com/v1/products", {
          name: this.name,
          price: this.price,
          description: this.description,
          category: this.category,
          color: this.color,
        }, {headers: {"Access-Control-Allow-Origin" : "*"}})
        .then(res => {
          console.log(res.data);
          alert("product saved");
          this.getProducts()
        })
        .catch(err => {
          console.log(err);
          alert("product couldnt save and error occured");
        });
    }
  },
  created() {
    this.getProducts();
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
