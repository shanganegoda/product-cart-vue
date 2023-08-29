<template>
    <header class="top-bar spread">
      <nav class="top-bar-nav">
        <router-link to="/" class="top-bar-link">
          <i class="icofont-spoon-and-fork"></i>
          <span>Home</span>
        </router-link>
        <router-link to="/products" class="top-bar-link">
          <span>Products</span>
        </router-link>
        <router-link to="/past-orders" class="top-bar-link">
          <span>Past Orders</span>
        </router-link>
      </nav>
      <a @click="toggleSideBar" class="top-bar-cart-link">
        <i class="icofont-cart-alt icofont-1x"></i>
        <span>Cart ({{ totalQuantity }})</span>
      </a>
    </header>
  <router-view :inventory="inventory" :addToCart="addToCart" />
   
   
  <Sidebar
  v-if="showSideBar" :toggle="toggleSideBar" :cart="cart" :inventory="inventory" :remove="removeItem"
  />
  
</template>

<script>
import Sidebar from '@/components/Sidebar.vue'
import food from './food.json'
// @ is an alias to /src
export default {
  components: {Sidebar},
  data () {
    return {
      inventory: food,
      showSideBar: false,
      cart: {}
    }
  },
  methods: {
        addToCart(i, name) {
          if (!this.cart[name]) {
            this.cart[name] = 0;
          }
          console.log(this.cart)
          this.cart[name] += this.inventory[i].quantity
          //console.log(JSON.stringify(this.cart))
          this.inventory[i].quantity = 0;
        },

        toggleSideBar() {
          this.showSideBar = !this.showSideBar
        },

        removeItem(name) {
          delete this.cart[name];

        }
      },

      computed: {
        totalQuantity() {
          return Object.values(this.cart).reduce((total, current) => {
            return total += current;
          }, 0)
        }
      }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
