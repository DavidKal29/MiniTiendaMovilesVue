<template>
    <section id="principal">
        <div id="header">
            <button><router-link to="/"><h1 id="titulo">MobileHub</h1></router-link></button>
        </div>
    </section>
</template>

<script>
export default {
    name:'CartView'
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