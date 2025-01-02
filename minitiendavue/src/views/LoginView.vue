<template>
    <section id="principal">
        <h1 id="titulo">MobileHub</h1>
        <div id="formu">
            <h1>Login</h1>
            <div class="input">
                <label>Usuario</label>
                <input type="text" placeholder="Introduce tu usuario" v-model="usuario">
            </div>
            <div class="input">
                <label>Contraseña</label>
                <input type="password" placeholder="Introduce tu contraseña" v-model="password">
            </div>
            <button @click="entrar()">Iniciar sesión</button>
            <p>¿No tienes cuenta? <router-link to="/register">Registrarme</router-link></p>
        </div>
    </section>
</template>

<script>
import { users } from '../db/db.js'
export default {
    name:'LoginView',
    data(){
        return {
            usuario:'',
            password:'',
            usuarios:[]
        }
    },
    methods:{
        getLocalStorage(){
            this.usuarios=JSON.parse(localStorage.getItem('users'))
            if (this.usuarios==null) {
                this.usuarios=users
                localStorage.setItem('users',JSON.stringify(this.usuarios))
            }
        }
    },
    mounted(){
        this.getLocalStorage()
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
        background-color: #007BFF
        gap: 30px
        #titulo
            font-weight: bold
            font-size: 40px
            color: white
        
        #formu
            display: flex
            justify-content: space-evenly
            align-items: center
            flex-direction: column
            border: 1px solid
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