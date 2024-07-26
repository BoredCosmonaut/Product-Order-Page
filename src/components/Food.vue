<template>

    <main>
        <div id="food-container">
            <div id="food-image-container" >
                <img v-if="windowWidth.value < 500" :src="food.image.mobile" alt="" id="food-image">
                <img v-else :src="food.image.desktop" id="food-image">
                <div v-if="quantity > 0" id="quantity-controls">
                    <img src="../../public/assets/images/icon-decrement-quantity.svg" alt="" @click="decreaseQuantity" class="quantity-button" id="decrease">
                    <span id="quantity">{{ quantity }}</span>
                    <img src="../../public/assets/images/icon-increment-quantity.svg" alt="" @click="increaseQuantity" class="quantity-button" id="increase">
                </div>
                <button v-else @click="addToCart" id="cart-button">
                    <img src="../../public/assets/images/icon-add-to-cart.svg" alt="" id="cart-image">
                    <p id="button-text">Add to cart</p>
                </button>    
            </div>

            <div id="food-text">
                <p id="food-category">{{ food.category }}</p>
                <p id="food-name">{{ food.name }}</p>
                <p id="food-price">${{ food.price }}</p>
            </div>
        </div>

    </main>
</template>

<script setup >
    import { ref,defineProps,defineEmits,onMounted,onUnmounted } from 'vue';

    const props = defineProps({
        food:Object,
        cart:Array
    });

    const quantity =ref(0);
    const emit = defineEmits(['update-cart']);
    const addToCart = (item) => {
        quantity.value = 1;
        emit('update-cart', { ...props.food, quantity: quantity.value });
    };

    const increaseQuantity = () => {
        quantity.value += 1;
        emit('update-cart', { ...props.food, quantity: quantity.value });
    }

    const decreaseQuantity = () => {
        if(quantity.value > 0) {
            quantity.value -=1;
            emit('update-cart', {...props.food,quantity: quantity.value})
            if(quantity.value === 0) {
                emit('update-cart',{...props.food,quantity:0});
            }
        }
    }

    const windowWidth = ref(window.innerWidth);

    const updateWindowWidth = () => {
    windowWidth.value = window.innerWidth;
    };

    onMounted(() => {
    window.addEventListener('resize', updateWindowWidth);
    });

    onUnmounted(() => {
    window.removeEventListener('resize', updateWindowWidth);
    });

</script>

<style scoped>
    main {
        background-color: hsl(20, 50%, 98%);
        width: 100%;
    }

    #dessert {
        margin-top: -1%;
    }

    #food-container {
        position: relative;
        left: 45px;
    }

    #food-image-container{
        width: 80%;
        display: flex;
        flex-direction: column;
    }

    #food-image {
        border-radius: 10px;
    }

    #cart-image {
        position: relative;
        top: 10px;
        margin-left: 10%;
    }

    #cart-button {
        width: 160px;
        height: 40px;
        padding: 0px 10px;
        border-radius: 20px;
        background-color: white;
        border: thin solid hsl(14, 65%, 9%);
        align-self: center;
        position: relative;
        display: flex;
        flex-direction: row;
        bottom: 20px;
    }

    #cart-button:hover {
        cursor: pointer;
        border-color: red;
    }

    #button-text {
        font-weight: 600;
        position: relative;
        bottom: 0px;
        left: 5px;
    }

    #quantity-controls {
        width: 140px;
        height: 38.5px;
        padding: 0px 10px;
        border-radius: 20px;
        background-color: red;
        border: thin solid hsl(14, 65%, 9%);
        align-self: center;
        position: relative;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        bottom: 20px;  
    }

    .quantity-button:hover{
        cursor: pointer;
        background-color: rgb(162, 0, 0);
    }

    #decrease {
        width: 10px;
        height: 10px;
        padding: 5px;
        border: solid white 2px;
        border-radius: 50%;
        margin-top: 5%;
    }

    #quantity {
        font-size: 18px;
        font-weight: 700;
        margin-top: 6%;
        color: white;
    }

    #increase {
        width: 10px;
        height: 10px;
        padding: 5px;
        border: solid white 2px;
        border-radius: 50%;
        margin-top: 5%
    }

    #food-text {
        margin-top: -5%;
    }

    #food-category {
        font-size: 13px;
        color: hsl(7, 20%, 60%);
        margin-bottom: -2%;
    }

    #food-name {
        font-size: 18px;
        margin-bottom: -2%;
    }

    #food-price {
        font-size: 18px;
        color: hsl(14, 86%, 42%)
    }

    @media(min-width:500px) {
        main {
            display: flex;
            width: 100%;
        }

        #food-container {
            display: flex;
            flex-direction: column;
            left: 0;
            width: 100%;
        }
    }
</style>