<template>
  <section id="principal">
    <div id="header">
        <button><router-link to="/"><h1 id="titulo">MobileHub</h1></router-link></button>
        <button @click="carrito()"><i class="fa-solid fa-cart-shopping"></i></button>
    </div>
    <div id="moviles">
        <div id="fila1" class="fila">
            <div v-bind:id="`movil${movil.id}`" class="movil" v-for="(movil,index) in moviles.slice(0,3)" :key="index">
                <div class="imagen_div">
                    <img v-bind:src="movil.image">
                </div>
                
                <span id="name">{{ movil.name }}</span>
                <span id="brand">{{ movil.brand }}</span>
                <span id="stock">Stock: {{ movil.stock }}</span>
                <span id="price">{{ movil.price }}€</span>

                <button @click="agregar(movil.id)" class="visible">Añadir al carrito</button>
            </div>
        </div>

        <div id="fila2" class="fila">
            <div v-bind:id="`movil${movil.id}`" class="movil" v-for="(movil,index) in moviles.slice(3,6)" :key="index">
                <div class="imagen_div">
                    <img v-bind:src="movil.image">
                </div>
                
                <span id="name">{{ movil.name }}</span>
                <span id="brand">{{ movil.brand }}</span>
                <span id="stock">Stock: {{ movil.stock }}</span>
                <span id="price">{{ movil.price }}€</span>
                <button @click="agregar(movil.id)" class="visible">Añadir al carrito</button>
        </div>
        </div>

        <div id="fila3" class="fila">
            <div v-bind:id="`movil${movil.id}`" class="movil" v-for="(movil,index) in moviles.slice(6,9)" :key="index">
                <div class="imagen_div">
                    <img v-bind:src="movil.image">
                </div>
                
                <span id="name">{{ movil.name }}</span>
                <span id="brand">{{ movil.brand }}</span>
                <span id="stock">Stock: {{ movil.stock }}</span>
                <span id="price">{{ movil.price }}€</span>
                <button @click="agregar(movil.id)" class="visible">Añadir al carrito</button>
        </div>
        </div>
    </div>
  </section>
</template>

<script>
import { mobileList, carrito, users } from '../db/db.js'
export default {
    name:'ProductsView',
    data(){
        return{
            moviles:[],
            cart:[],
            usuarios:[],
            userCarrito:''
        }
    },
    methods:{
        getLocalStorageCart(){//Obtener el carrito del localstorage
            this.cart=JSON.parse(localStorage.getItem('cart'))
            if (this.cart==null) {
                this.cart=carrito//si no existe, le asignamos a un array vacio de db.js
                localStorage.setItem('cart',JSON.stringify(this.cart))
            }
            console.log(this.cart)
        },
        getLocalStorageUsers(){//Obtener los usuarios del localstorage
            this.usuarios=JSON.parse(localStorage.getItem('users'))
            if (this.usuarios==null) {
                this.usuarios=users//si no existe, le asignamos a un array vacio de db.js
                localStorage.setItem('users',JSON.stringify(this.usuarios))
            }
        },
        getLocalStorageMobiles(){//Obtener los moviles del localstorage
            this.moviles=JSON.parse(localStorage.getItem('moviles'))
            if (this.moviles==null) {
                this.moviles=mobileList//si no existe, le asignamos a un array vacio de db.js
                localStorage.setItem('moviles',JSON.stringify(this.moviles))
            }
        },
        carrito(){//dirigimos al usuario a su carrito
            this.$router.push({name:'cart',params:{'username':this.$route.params.username}})
        },
        agregar(index){//funcion par agregar productos al carrito
            let user=this.usuarios.find(u=>u.username==this.$route.params.username)//obtenemos el user que está comprando
            console.log(user);
                    
            let product=this.moviles.find(p=>p==this.moviles[index])//obtenemos el producto que ha querido añadir el user
            product=structuredClone(product)//creamos una copia de ese producto independiente para manipularlo
            console.log(product);

            if (product.stock==0) {//si no hay stock de ese producto
                alert('No quedan más unidades de este producto')//mostramos el mensaje de error
            }else{
                if (this.cart.length==0 || this.userCarrito=='') {//el carrito está vacio o el usuario no tiene nada en el carrito
                    product.counter=1//creamos un contador para el producto
                    this.moviles[index].stock-=1//quitamos uno de stock de ese producto
                    localStorage.setItem('moviles',JSON.stringify(this.moviles))//actualizamos el array de moviles en el localstorage
                    
                    let pedido={user:user,products:[product]}//creamos el pedido
                    this.cart.push(pedido)//añadimos el pedido al carrito
                    localStorage.setItem('cart',JSON.stringify(this.cart))//acutalizamos el carrito en el localstorage
                    console.log(this.cart);
                    this.userCarrito=pedido//hacemos que al usuario esté asociado algun pedido
                }else{//si el usuario ya tiene cosas en el carrito
                    let productoCarrito=this.userCarrito.products.find(p=>p.name==product.name)
                    if (!productoCarrito) {//el producto no ha sido añadido al carrito por el user
                        product.counter=1//creamos un contador para el producto
                        this.moviles[index].stock-=1//quitamos uno de stock de ese producto
                        localStorage.setItem('moviles',JSON.stringify(this.moviles))//actualizamos el array de moviles en el localstorage
                        this.userCarrito.products.push(product)//añadimos al pedido en la lista de productos, el nuevo producto
                            
                        localStorage.setItem('cart',JSON.stringify(this.cart))//acutalizamos el carrito en el localstorage
                        console.log(this.cart);
                    }else{//el producto si ha sido añadido al carrito por el user
                        productoCarrito.counter+=1//incrementamos la cantidad de ese producto
                        this.moviles[index].stock-=1//quitamos uno de stock de ese producto
                        localStorage.setItem('moviles',JSON.stringify(this.moviles))//actualizamos el array moviles
                        localStorage.setItem('cart',JSON.stringify(this.cart))//y el carrito en el localstorage
                        console.log(this.cart);
                    }
                }
            }
            

            alert('Añadido el producto')//le mostramos al user el mensaje de éxito
        }
    },
    mounted(){
        //hacemos todo el proceso de obtencion de los datos del localstorage
        this.getLocalStorageUsers()
        this.getLocalStorageCart()
        this.getLocalStorageMobiles()
        
        //comprobamos que el usuario esta en el carrito
        //Esto es para saber si el usuario ya tiene cosas metidas en el carrito
        //Con esto, sabremos que hacer en la funcion de agregar al carrito

        this.userCarrito=this.cart.find(u=>u.user.username==this.$route.params.username)

        if (!this.userCarrito) {
            this.userCarrito=''//si el usuario no ha metido nada en el carrito, simplemente hacemos que la variable sea un ''
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