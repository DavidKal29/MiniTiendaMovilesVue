<template>
    <section id="principal">
        <div id="header">
            <button><router-link to="/"><h1 id="titulo">MobileHub</h1></router-link></button>
        </div>
        <div id="sumarios">
            <div id="sinProductos" v-if="this.productos.length==0">
                    <h1>No tienes productos en el carrito</h1>
            </div>
            <div id="productos" v-if="this.productos.length!=0">
                <div class="producto" v-for="(movil,index) in productos" :key="index">
                    <div class="info">
                        <div class="imagen_div">
                            <img v-bind:src="movil.image" alt="">
                        </div>

                        <div class="titulo_counter">
                            <h1>{{ movil.name }}</h1>
                            <div class="botones">
                                <button @click="decrementar(movil.id,index)"><i class="fa-solid fa-circle-minus"></i></button>
                                <span>{{ movil.counter }}</span>
                                <button @click="incrementar(movil.id)"><i class="fa-solid fa-circle-plus"></i></button>
                            </div>
                        </div>
                    </div>

                    <div class="quitar_precio">
                        <button @click="quitar(movil.id,index)"><i class="fa-solid fa-x"></i></button>
                        <span>{{ movil.price }}€</span>
                    </div>

                </div>
                

                
            </div>




            <div id="sumario">
                <h1>Sumario</h1>
                <div class="precios">
                    <div class="precio">
                        <span class="subtitulo">Subtotal</span>
                        <span>{{ this.precioTotal }}€</span>
                    </div>

                    <div v-if="this.precioTotal!=0" class="precio">
                        <span class="subtitulo">Envío</span>
                        <span>19.99€</span>
                    </div>
                </div>  
                <hr>
                <div class="precio">
                    <span class="subtitulo">Total</span>
                    <span v-if="this.precioTotal!=0">{{ this.precioTotal+19.99 }}€</span>
                    <span v-if="this.precioTotal==0">0€</span>
                </div>
                <button @click="comprar()">Finalizar compra</button>  
            </div>
        </div>

        
       
    </section>
</template>

<script>
import { mobileList, carrito, users } from '../db/db.js'
export default {
    name:'CartView',
    data(){
        return {
            moviles:[],
            cart:[],
            usuarios:[],
            userCarrito:'',
            productos:[],
            precioTotal:0
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
        calcularPrecio(){//Funcion para saber cuanto cuesta todo lo que hemos agregado al carrito
            if (this.productos.length!=0) {//si tenemos productos
                this.precioTotal=0//hacemos que el precio sea 0
                this.productos.forEach(p => {//recorremos todos los productos que hemos agregado al carrito
                    let precio=p.counter*p.price//multiplicamos la cantidad por el precio de ese producto
                    this.precioTotal+=precio//lo sumamos al total 
                });
            }else{//si no tenemos productos
                this.precioTotal=0//el precio es 0 y ya
            }
            
        },
        incrementar(id){//funcion para aumentar el contador de un producto en el carrito
            if (this.moviles.find(p=>p.id==id).stock==0) {//si el stock de ese producto es 0
                alert('No quedan mas unidades de ese producto')//notificamos que no podemos comprar mas porque no quedan
            }else{//si aun queda stock
                this.moviles.find(p=>p.id==id).stock-=1//le quitamos uno de stock al producto del array de los moviles de db
                localStorage.setItem('moviles',JSON.stringify(this.moviles))//actualizamos el localstorage de esos moviles
                this.productos.find(p=>p.id==id).counter+=1//añadimos uno al contador del producto en nuestro carrito
                localStorage.setItem('cart',JSON.stringify(this.cart))//actualizamos el carrito en el localstorage 
                this.calcularPrecio()//calculamos el precio para que aparezca en el sumario actualizado
            }
        },
        decrementar(id,index){//funcion para decrementar el contador de un producto en el carrito
            this.moviles.find(p=>p.id==id).stock+=1//aumentamos uno de stock al producto del array de los moviles de db
            localStorage.setItem('moviles',JSON.stringify(this.moviles))//actualizamos el localstorage de esos moviles
            
            this.productos.find(p=>p.id==id).counter-=1//quitamos uno del contador del producto en el carrito
            localStorage.setItem('cart',JSON.stringify(this.cart))//actualizamos los cambios en el carrito en el localstorage
            this.calcularPrecio()//calculamos el precio para que aparezca en el sumario actualizado
            if (this.productos.find(p=>p.id==id).counter==0) {//si el contador es 0
                this.productos.splice(index,1)//quitamos de nuestros productos del carrito, ese producto
                localStorage.setItem('cart',JSON.stringify(this.cart))//actualizamos de nuevo el localstorage del carrito
            }
        },
        quitar(id,index){//funcion para quitar directamente un producto del carrito
            let stock=this.productos.find(p=>p.id==id).counter//obtenemos la cantidad que hemos añadido de un producto al carrito
            this.moviles.find(p=>p.id==id).stock+=stock//le sumamos al producto del array de los moviles de la db, esa cantidad
            localStorage.setItem('moviles',JSON.stringify(this.moviles))//actualizamos el localstorage de los moviles
            this.productos.find(p=>p.id==id).counter-=stock//le quitamos al producto del carrito toda su cantidad

            this.calcularPrecio()//calculamos otra vez el precio para que en el sumario salga el precio actualizado 
            
            this.productos.splice(index,1)//quitamos el producto de los productos del carrito
            localStorage.setItem('cart',JSON.stringify(this.cart))//actualizamos el carrito del localtorage
            
            

        },
        comprar(){//funcion para comprar ya los productos que tenemos en el carrito
            if (this.productos.length!=0) {//si tenemos productos en el carrito, osea que el length de nuestros productos no es 0
                for (let i = 0; i < this.cart.length; i++) {//recorremos el carrito entero
                    if (this.cart[i]==this.userCarrito) {//si el pedido de ese carrito coincide con el nuestro
                        this.cart.splice(i,1)//borramos del carrito nuestro pedido
                        this.userCarrito=''//nuestro pedido ahora es nulo, osea, no hemos añadido nada al carrito
                        this.productos=[]//los productos añadidos al carrito se vuelven un array vacio, porque ya no tenemos ninguno
                        localStorage.setItem('cart',JSON.stringify(this.cart))  //actualizamos el carrito en el localstorage
                    }
                }

                alert('Compra realizada con éxito')//mostramos mensaje de exito
                this.calcularPrecio()//calculamos el precio para que salga en el sumario, en este caso, será 0 
            }else{//si no tenemos productos en el carrito
                alert('No tienes productos')//advertimos que no se puede realizar la compra
            }
            
        }
    },
    mounted(){//funcion que se ejecutara nada mas entrar en la seccion 
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
        }else{//sino
            this.productos=this.userCarrito.products//añadimos a los productos, los productos asociados a nuestro pedido
            console.log('Productos:',this.productos);

            this.calcularPrecio()//calculamos el precio, para que salga correctamente en el sumario
        }

        

        

        console.log(this.userCarrito);
        

    }
}
</script>

<style lang="sass" scoped>
    #principal
        display: flex
        justify-content: start
        align-items: center
        flex-direction: column
        height: 100vh
        background-color: #AED6F1
        gap: 30px
        padding: 20px
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
                font-weight: bold
                font-size: 40px
        #sumarios
            display: flex
            justify-content: center
            align-items: center
            gap: 40px
            #sumario
                display: flex
                justify-content: start
                align-items: start
                flex-direction: column
                background-color: white
                border-radius: 8px
                width: 45vh
                height: 45vh
                gap: 35px
                padding: 20px
                margin-bottom: 250px
                .precios
                    display: flex
                    justify-content: center
                    align-items: start
                    flex-direction: column
                    gap: 35px
                .precio
                    display: flex
                    justify-content: space-between
                    align-items: center
                    width: 40vh
                    .subtitulo
                        color: #007BFF
                        font-weight: bold
                hr
                    height: 3px
                    width: 100%
                    background-color: #007BFF
                    border: none
                    border-radius: 5px
                button
                    padding: 10px 99px 
                    color: white
                    border: none
                    border-radius: 5px
                    background-color: #007BFF
                    font-weight: bold
                    cursor: pointer
            
            #sinProductos
                display: flex
                justify-content: start
                align-items: center
                flex-direction: column
                gap: 20px
                width: 110vh
                height: 80vh
                padding: 15px
                background-color: white
                h1
                    margin-top: 270px
            #productos,#sinProductos
                display: flex
                justify-content: start
                align-items: center
                flex-direction: column
                gap: 20px
                width: 110vh
                height: 80vh
                padding: 15px
                overflow-y: scroll
                .producto
                    display: flex
                    justify-content: space-between
                    align-items: center
                    background-color: white
                    border-radius: 8px
                    border: 3px solid #007BFF
                    width: 100vh
                    height: 20vh
                    padding: 25px
                    .info
                        display: flex
                        justify-content: center
                        align-items: center
                        gap: 12px
                        .imagen_div
                            width: 13vh
                            height: 16vh
                            //margin-left: 50px
                            img
                                width: 100%
                                height: 100%
                        .titulo_counter
                            display: flex
                            justify-content: space-between
                            align-items: start
                            flex-direction: column
                            height: 15vh
                            .botones
                                display: flex
                                justify-content: center
                                align-items: center
                                gap: 10px
                                button
                                    background: none
                                    border: none
                                    color: #007BFF
                                    font-size: 25px
                                    cursor: pointer
                                span
                                    color: #007BFF
                                    font-size: 20px
                                    font-weight: bold

                    .quitar_precio
                        display: flex
                        justify-content: space-between
                        align-items: end
                        flex-direction: column
                        height: 15vh
                        button
                            border: none
                            border-radius: 50%
                            height: 30px
                            width: 30px
                            background-color: red
                            color: white
                            text-align: center
                            i
                                font-size: 12px
                        span
                            color: #007BFF
        
                    
                

                
                    


                        


        



</style>