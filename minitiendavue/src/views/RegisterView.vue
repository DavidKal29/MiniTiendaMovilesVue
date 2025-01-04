<template>
    <section id="principal">
        <div id="header">
            <h1 id="titulo">MobileHub</h1>
            <button><i class="fa-solid fa-cart-shopping"></i></button>
        </div>
        <div id="formu">
            <h1>Register</h1>
            <div class="input">
                <label>Nombre</label>
                <input type="text" placeholder="Introduce tu nombre" v-model="name">
            </div>
            <div class="input">
                <label>Usuario</label>
                <input type="text" placeholder="Introduce tu usuario" v-model="username">
            </div>
            <div class="input">
                <label>Contraseña</label>
                <input type="password" placeholder="Introduce tu contraseña" v-model="password">
            </div>
            <button @click="crear()">Crear cuenta</button>
            <p>¿Ya tienes cuenta? <router-link to="/">Iniciar sesión</router-link></p>
        </div>
    </section>
</template>

<script>
import { users } from '../db/db.js'
export default {
    name:'RegisterView',
    data(){
        return{
            name:'',
            username:'',
            password:'',
            usuarios:[]
        }
    },
    methods:{
        getLocalStorageUsers(){//Obtener los usuarios del localstorage
            this.usuarios=JSON.parse(localStorage.getItem('users'))
            if (this.usuarios==null) {
                this.usuarios=users//si no existe, le asignamos a un array vacio de db.js
                localStorage.setItem('users',JSON.stringify(this.usuarios))
            }
        },
        crear(){//funcion para crear al usuario
            if (this.name=='' || this.username=='' || this.password=='') {//comprueba que el usuario relleno todos los campos
                alert('Rellena todos los campos')//mensaje de eror
            }else{
                let user={//crea el objeto que será metido en el array de users
                    name:this.name,
                    username:this.username,
                    password:this.password
                }

                this.usuarios.push(user)//mete el user al array de usuarios
                localStorage.setItem('users',JSON.stringify(this.usuarios))//actualiza el localstorage de los usuarios
                console.log(this.usuarios);  

                //borra lo introducido en los inputs
                this.name=''
                this.username=''
                this.password=''

                alert('Cuenta creada con éxito')
            }
        }
    },
    mounted(){
        this.getLocalStorageUsers()//actualiza el localstorage de los usuarios
    }
    
}
</script>

<style lang="sass" scoped>
    #principal
        display: flex
        justify-content: center
        align-items: center
        flex-direction: column
        height: 100vh
        background-color: #AED6F1
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
                font-weight: bold
                font-size: 40px
        #formu
            display: flex
            justify-content: space-evenly
            align-items: center
            flex-direction: column
            border-radius: 5px
            width: 50vh
            height: 60vh
            background-color: white
            h1
                font-weight: bold
                font-size: 30px
            .input
                display: flex
                justify-content: center
                align-items: start
                flex-direction: column
                gap: 2px
                input
                    width: 35vh
                    padding: 8px 5px
                    border-radius: 5pxs
                    outline: none
                label
                    font-weight: bold
            button
                padding: 8px 16px 
                color: white
                border: none
                border-radius: 5px
                background-color: #007BFF
                font-weight: bold
                cursor: pointer
            a
                text-decoration: none
                color: #007BFF




</style>