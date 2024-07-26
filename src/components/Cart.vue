<template>
    <main id="card">
        <div id="cart" >
            <h2 id="item-amount" >Your Cart ({{ itemQuantity }})</h2>
        </div>
        <div id="cart-inside" >
            <div v-if="items.length === 0" id="empty-cart-inside">
                <div id="empty-cart-image" >
                    <img src="../../public/assets/images/illustration-empty-cart.svg" alt="" id="empty-cart-image">
                </div>
                <p id="empty-cart-text" >Your added items will be shown here</p>
            </div>
            <div v-else id="full-cart" >
                <div id="cart-item" v-for="item in items" :key="item.name">
                    <div id="item-detail">
                        <p id="item-name">{{ item.name }}</p>
                        <p id="item-price"><span id="item-quantity">{{ item.quantity }}x </span><span id="item-price">@{{ item.price }}</span><span id="total-price"> ${{(item.price * item.quantity).toFixed(2)}}</span></p>
                    <hr class="line">
                    </div>
                    <div id="remove-button-container">
                        <img src="../../public/assets/images/icon-remove-item.svg" alt="" id="remove-item-image" @click="removeFromCart(item.name)">
                    </div>
                </div>
                <div id="cart-summary">
                    <p id="total-order">Order Total <span id="bot-total-price">${{ totalPrice.toFixed(2) }}</span></p>
                    <p id="carbon"><img src="../../public/assets/images/icon-carbon-neutral.svg" alt="" id="carbon-tree">This is a <span id="bold-carbon">carbon-neutral</span> delivery</p>
                    <button id="confirm" @click="confirmOrder">Confirm Order</button>
                </div>
            </div>
        </div>

        <div id="order-conformation-overlay" v-if="showConformation">
            <div id="conformation-inside">
                <div id="conformation-inside-top">
                    <div id="checkmark-container">
                        <img src="../../public/assets/images/icon-order-confirmed.svg" alt="" id="checkmark-image">
                    </div>
                    <h2 id="order-header">Order Confirmed</h2>
                    <p id="con-top-text">We hope you enjoy your food!</p>
                </div>
                <div id="con-order-details">
                    <div v-for="item in items" :key="item.name" id="order-item">
                        <div id="order-item-image-container">
                            <img :src="item.image.thumbnail" alt="" id="order-item-image">
                        </div>
                        <div id="order-item-detail">
                            <div id="detail-mid">
                                <p id="order-item-name">{{ item.name }}</p>
                                <p id="order-item-quantity">{{ item.quantity }}x <span id="con-item-price">@${{ item.price }}</span></p>
                                <hr class="con-line">
                            </div>
                            <p id="order-item-price">${{ (item.quantity * item.price).toFixed(2) }}</p>
                        </div>
                    </div>
                    <div id="con-order-total">
                        <p id="con-order-total-text"><span id="con-total-text">Order Total</span> <span id="con-total-price">${{ totalPrice.toFixed(2) }}</span></p>
                    </div>
                </div>
                <button @click="startNewOrder" id="new-order-button">Start New Order</button>
            </div>
        </div>
    
    </main>
</template>

<script setup>
    import { computed, defineProps,defineEmits,ref } from 'vue';

    const props = defineProps({
        items:Array,
    });

    const emit = defineEmits(['remove-from-cart']);
    const removeFromCart = (name) => {
        emit('remove-from-cart',name);
    };

    const itemQuantity = computed(() => {
        return props.items.reduce((sum,item) => sum + item.quantity,0);
    });

    const totalPrice = computed(() => {
        return props.items.reduce((sum, item) => sum + item.quantity * item.price, 0);
    });


    const showConformation = ref(false);

    const confirmOrder = () => {
        showConformation.value = true;
        document.body.style.overflow = 'hidden';
    }

    const startNewOrder = () => {
        emit('empty-cart');
        showConformation.value = false;
        document.body.style.overflow = '';
    }

</script>

<style scoped>
    #card {
        background-color: white;
        width: 350px;
        margin-left: 50px;
        margin-top: -10%;
    }

    #cart {
        text-align: left;
    }

    #item-amount {
        font-weight: 900;
        font-size: 25px;
        color: red;
        margin-left: 5%;
    }

    #cart-inside {
        display: flex;
        flex-direction: row;
        padding-bottom: 20px;
        padding-right: 10px;
        padding-left: 10px;
    }

    #empty-cart-inside {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    #full-cart {
        width: 100%;
        margin-top: -7%;
        margin-left: 3%;
    }

    #cart-item {
        display: flex;
        flex-direction: row;
        width: 100%;
    }

    #item-detail {
        width: 90%;
    }

    #item-name {
        font-weight: bold;
        font-size: 14px;
    }

    #item-quantity {
        color: red;
        font-weight: 600;
        margin-left: -6%;
        font-size: 14px;
    }

    #item-price {
        margin-left: 7%;
        font-size: 14px;
        color: hsl(7, 20%, 60%);
    }

    #total-price {
        margin-left: 7%;
        font-size: 14px;
        color: hsl(12, 20%, 44%);
    }

    #remove-button-container {
        width: 10%;
    }

    #remove-item-image {
        margin-top: 60%;
        width: 15px;
        height: 15px;
        padding: 3px;
        margin-left: auto;
        margin-right: 5%;
        border: solid 1px hsl(14, 25%, 72%);
        border-radius: 50%;
    }

    #remove-item-image:hover {
        cursor: pointer
    }

    .line {
        width: 110%;
    }

    #cart-summary {
        display: flex;
        flex-direction: column;
        width: 100%;
    }

    #total-order {
        font-size: 15px;
        font-weight: 500;
    }

    #bot-total-price {
        font-size: 24px;
        font-weight: 700;
        margin-left: 55%;
    }

    #carbon {
        background-color: hsl(20, 50%, 98%);
        text-align: center;
        padding: 5px 10px;
        border-radius: 10px;
    }

    #bold-carbon {
        font-weight: bold;
    }

    #confirm {
        width: 80%;
        align-self: center;
        padding: 10px 5px;
        background-color: red;
        border: none;
        border-radius: 15px;
        color: white;
        font-weight: 700;
    }

    #confirm:hover {
        cursor:pointer;
        background-color: rgb(198, 2, 2)
    }

    #order-conformation-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 1000;
    }

    #conformation-inside {
        background: white;
        border-radius: 10px;
        width: 450px;
        text-align: left;
        padding-bottom: 35px;
    }

    #confotmation-inside-top {
        display: flex;
        flex-direction: column;
        width: 100%;
    }

    #checkmark-image {
        margin-top: 5%;
        margin-left: 5%;
        width: 50px;
    }

    #order-header {
        margin-top: -2%;
        margin-left: 5%;
        width: 30%;
        font-size: 35px; 
    }

    #con-top-text {
        color: hsl(7, 20%, 60%);
        margin-left: 5%;
        margin-top: -5%;
        font-size: 15px;
    }

    #con-order-details {
        display: flex;
        flex-direction: column;
        padding: 15px;
        width: 80%;
        margin-left: 5%;
        align-self: center;
        background-color: hsl(20, 50%, 98%);
    }

    #order-item {
        display: flex;
        flex-direction: row;
        width: 100%;
    }

    #order-item-image {
        width: 75px;
        height: 75px;
        border-radius: 10px
    }

    #order-item-detail {
        display: flex;
        flex-direction: row;
        width: 100%;
    }

    #detail-mid {
        width: 100%;
    }

    #order-item-name {
        font-size: 15px;
        font-weight: 700;
        margin-left: 5%;
    }

    #order-item-quantity {
        font-size: 15px;
        font-weight: 700;
        margin-left: 5%;
        color: red;
    }

    #con-item-price {
        margin-left: 5%;
        font-size: 15px;
        font-weight: 700;
        color: hsl(14, 25%, 72%);
    }

    #order-item-price {
        margin-top: 10%;
        margin-left: 5%;
        font-size: 15px;
        font-weight: 700;
    }

    .con-line {
        position: relative;
        width: 157%;
        right: 35%;
    }

    #con-order-total-text {
        font-size: 14px;
        font-weight: 600;
        color: hsl(14, 25%, 72%);
    }

    #con-total-text {
        position: relative;
        bottom: 5px;
    }

    #con-total-price {
        font-size: 30px;
        font-weight: 700;
        color: black;
        margin-left: 215px;
    }

    #new-order-button {
        display: flex;
        flex-direction: column;
        text-align: center;
        position: relative;
        top: 10px;
        width: 60%;
        background-color: hsl(14, 86%, 42%);
        border: none;
        padding: 10px 5px;
        border-radius: 15px;
        color: white;
        margin: auto;
        justify-content: center;
        justify-self: center;
        align-items: center;
        align-self:center; 
    }

    #new-order-button:hover{
        cursor: pointer;
        background-color: rgb(198, 2, 2)
    }

    @media(min-width:500px) {
        #card {
            width: 100%;
            margin-left: 0;
        }

        #bot-total-price {
            font-size: 20px;
            margin-left: 45%;
        }

        #conformation-inside {
            width: 40%;
        }

        .con-line {
            position: relative;
            width: 170%;
            max-width: 475px;
            left: -75px;
        }

        #order-item-price {
        margin-top: 10%;
        margin-right: 5%;
        font-size: 15px;
        font-weight: 700;
        }

        #con-total-price {
            font-size: 20px;
            margin-left: 60%;
            position: relative;
            bottom: 5px;
        }
    }
</style>