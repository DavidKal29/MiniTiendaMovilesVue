<template>
  <section id="principal">
    <div id="header">
        <button><router-link to="/"><h1 id="titulo">MobileHub</h1></router-link></button>
        <button @click="carrito()"><i class="fa-solid fa-cart-shopping"></i></button>
    </div>
    <div id="moviles">
        <div id="fila1" class="fila">
            <div v-bind:id="`movil${movil.id}`" class="movil" v-for="(movil) in moviles.slice(0,3)" :key="movil.id">
                <div class="imagen_div">
                    <img v-bind:src="movil.image">
                </div>
                
                <span id="name">{{ movil.name }}</span>
                <span id="brand">{{ movil.brand }}</span>
                <span id="price">{{ movil.price }}€</span>
                <button>Añadir al carrito</button>
            </div>
        </div>

        <div id="fila2" class="fila">
            <div v-bind:id="`movil${movil.id}`" class="movil" v-for="(movil) in moviles.slice(3,6)" :key="movil.id">
                <div class="imagen_div">
                    <img v-bind:src="movil.image">
                </div>
                
                <span id="name">{{ movil.name }}</span>
                <span id="brand">{{ movil.brand }}</span>
                <span id="price">{{ movil.price }}€</span>
                <button>Añadir al carrito</button>
            </div>
        </div>

        <div id="fila3" class="fila">
            <div v-bind:id="`movil${movil.id}`" class="movil" v-for="(movil) in moviles.slice(6,9)" :key="movil.id">
                <div class="imagen_div">
                    <img v-bind:src="movil.image">
                </div>
                
                <span id="name">{{ movil.name }}</span>
                <span id="brand">{{ movil.brand }}</span>
                <span id="price">{{ movil.price }}€</span>
                <button id="agregar" @click="agregar()">Añadir al carrito</button>
            </div>
        </div>
    </div>
  </section>
</template>

<script>
import { mobileList } from '../db/db.js'
export default {
    name:'ProductsView',
    data(){
        return{
            moviles:mobileList
        }
    },
    methods:{
        carrito(){
            let username=this.$route.params.username
            this.$router.push({name:'cart',params:{'username':username}})
        }
    }
}
</script>

<style lang="sass" scoped>
    #principal
        background-color: #AED6F1
        display: flex
        justify-content: center
        align-items: center
        flex-direction: column
        padding: 50px
        gap: 30px
        #header
            display: flex
            justify-content: center
            align-items: center
            gap: 15px
            button
                border: none
                background: none
                cursor: pointer
                i
                    font-size: 35px
                a
                    text-decoration: none
                    color: black
            #titulo
                font-size: 40px
        #moviles
            display: flex
            justify-content: center
            align-items: center
            flex-direction: column
            gap: 50px
            .fila
                display: flex
                justify-content: center
                align-items: center
                gap: 120px
                .movil
                    display: flex
                    justify-content: center
                    align-items: center
                    flex-direction: column
                    gap: 6px
                    .imagen_div
                        width: 22vh
                        height: 25vh
                        img
                            width: 100%
                            height: 100%
                    #name
                        font-weight: bold
                        font-size: 20px
                    #brand
                        font-style: italic
                        color: gray
                    #price
                        color: #007BFF
                    button
                        padding: 8px 16px 
                        color: white
                        border: none
                        border-radius: 5px
                        background-color: #007BFF
                        //font-weight: bold
                        cursor: pointer
                    
                            


</style>