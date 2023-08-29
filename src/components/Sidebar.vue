/* eslint-disable */
<template>
    <aside class="cart-container">
    <div class="cart">
      <h1 class="cart-title spread">
        <span>
          Cart
          <i class="icofont-cart-alt icofont-1x"></i>
        </span>
        <button @click="toggle" class="cart-close">&times;</button>
      </h1>

      <div class="cart-body">
        <table class="cart-table">
          <thead>
            <tr>
              <th><span class="sr-only">Product Image</span></th>
              <th>Product</th>
              <th>Price</th>
              <th>Qty</th>
              <th>Total</th>
              <th><span class="sr-only">Actions</span></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(quantity, key, i) of cart" :key="i">
              <td><i class="icofont-carrot icofont-3x"></i></td>
              <td>{{key}}</td>
              <td>${{getPrice(key)}}</td>
              <td class="center">{{quantity}}</td>
              <td>${{(quantity * getPrice(key)).toFixed(2)}}</td>
              <td class="center">
                <button @click="remove(key)" class="btn btn-light cart-remove">
                  &times;
                </button>
              </td>
            </tr>
          </tbody>
        </table>

        <p v-if="isCartEmpty()" class="center"><em>No items in cart</em></p>
        <div class="spread">
          <span><strong>Total:</strong> {{calculateTotal()}}</span>
          <button class="btn btn-light">Checkout</button>
        </div>
      </div>
    </div>
  </aside>
</template>

<script>

export default ({
    props: ['toggle', 'cart', 'inventory', 'remove'],
    computed: {
        cartTotal() {
          return (this.cart.carrots * 4.82).toFixed(2)
        }
      },
      methods: {
        getPrice(name) {
          const product = this.inventory.find((p) => {
            return p.name === name
          })

          return Number(product.price.USD)
        },
        calculateTotal() {
          // let cartTotal = 0;

          let cartTotal = Object.entries(this.cart).reduce((prev, curr) => {
            return prev += (curr[1] * this.getPrice(curr[0]));
          }, 0)

          return cartTotal.toFixed(2);

          // let items = Object.keys(this.cart);
          // items.forEach(element => {
          //   cartTotal += this.cart[element] * this.getPrice(element);
          // });
          // return cartTotal;
        },
        isCartEmpty() {
          if (JSON.stringify(this.cart) === '{}') {
            return true;
          }
          return false;
        }
      }
})
</script>

