<template>
  <body>
    <main>
      <h1 id="dessert">Desserts</h1>
      <div id="components">
        <div id="food-container">
          <Food class="food" v-for="food in foods"  :key="food.name" :food ="food" @update-cart="updateCart"/>
        </div>
        <div id="cart-container">
          <Cart class="cart" :items="cartItems" @remove-from-cart="removeFromCart" @empty-cart ="emptyCart" />
        </div>
      </div>
    </main>
  </body>
</template>

<script setup>
  import { onMounted,ref } from 'vue';
  import Food from './components/Food.vue';
  import Cart from './components/Cart.vue';
  
  const foods = ref([]);
  const cartItems = ref([]);

  onMounted(async() => {
    const response = await fetch("/data.json");
    foods.value = await response.json();
    console.log(foods);
  });

  const updateCart = (item) => {
    const existingItem = cartItems.value.find(cartItem => cartItem.name === item.name);
    if(item.quantity > 0){
      if(existingItem) {
        existingItem.quantity = item.quantity;
      } else {
        cartItems.value.push({...item});
      }
    }
    else if(existingItem) {
      cartItems.value = cartItems.value.filter(cartItem => cartItem.name !== item.name);
    }
  };

  const removeFromCart = (name) => {
  cartItems.value = cartItems.value.filter(item => item.name !== name);
  };

  const emptyCart = () => {
    cartItems.value = [];
  }

</script>

<style scoped>
  body {
   	min-height: 100vh;
    display: grid;
    place-items: center;
  }

  main {
    width: 450px;
    height: auto;
    padding-bottom: 30px;
    display: flex;
    flex-direction: column;
    background-color: hsl(20, 50%, 98%);
  }

  #dessert {
    margin-top: -1%;
  }

  @media(min-width:500px) {
    body {
      background-color: hsl(20, 50%, 98%);
    }

    main {
      display: flex;
      flex-direction: column;
      width: 100%;
    }

    #components {
      width: 100%;
      display: flex;
      flex-direction: row;
    }

    #food-container {
      width: 80%;
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      margin-left: 5%;
    }

    #dessert {
      margin-top: 1%;
      margin-left: 5%;
    }

    #cart-container {
      width: 40%;
    }

    .food {
      width: 33%;
    }
  }
</style>
