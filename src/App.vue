<template>
  <div>
    <!-- Item Listings -->
    <div>
      <h2>Items</h2>
      <div v-for="item in products" :key="item.id" class="item-container">
        <div class="add-to-cart-btn">
          <button @click="addToCart(item)">Add to Cart</button>
        </div>
        <div class="item-info">
          <p>{{ item.name }} - ₱{{ item.price }}</p>
        </div>
      </div>
    </div>

    <!-- Shopping Cart -->
    <div>
      <h2>Cart</h2>
      <div v-if="cart.length === 0">Your cart is empty</div>
      <div v-else>
        <div v-for="(item, index) in cart" :key="index">
          <p>{{ item.product.name }} - ₱{{ item.product.price }} x {{ item.quantity }}</p>
          <p>Update</p>
          <button @click="decreaseQuantity(index)">-</button>&nbsp;
          <button @click="increaseQuantity(index)">+</button> &nbsp; | &nbsp;
          <button @click="removeFromCart(index)">Remove</button>
        </div>
        <p>Total: ₱{{ totalPrice }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Item 1', price: 10 },
        { id: 2, name: 'Item 2', price: 20 },
        { id: 3, name: 'Item 3', price: 30 }
      ],
      cart: []
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((total, item) => {
        return total + (item.product.price * item.quantity);
      }, 0);
    }
  },
  methods: {
    addToCart(item) {
      const foundIndex = this.cart.findIndex(cartItem => cartItem.product.id === item.id);
      if (foundIndex !== -1) {
        this.cart[foundIndex].quantity++;
      } else {
        this.cart.push({ product: item, quantity: 1 });
      }
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    },
    increaseQuantity(index) {
      this.cart[index].quantity++;
    },
    decreaseQuantity(index) {
      if (this.cart[index].quantity > 1) {
        this.cart[index].quantity--;
      }
    }
  }
};
</script>

<style scoped>
.item-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.item-info {
  flex-grow: 1;
}

.add-to-cart-btn {
  margin-right: 10px;
}
</style>

