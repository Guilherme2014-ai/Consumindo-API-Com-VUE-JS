<template>
    <div class="column is-half is-offset-one-quarter">



        <div class="card">


            <div class="card-image">
                <figure>
                    <img v-if="isFront == true" class="img" :src="pokemon.front" alt="Pokemon_PNG" @click="changeSide()" @mouseenter="hover()" @mouseleave="AfterHover()">
                    <img v-else :src="pokemon.back" class="img" alt="Pokemon_PNG" @click="changeSide()" @mouseenter="hover()" @mouseleave="AfterHover()">
                </figure>
            </div>

            <div class="card-content">
                <div class="media">
                    <div class="media-content">
                        <p class="title is-4">{{ num }} | {{ nameP | bigName}}</p>
                        <p class="subtitle is-6">{{ pokemon["type"] }}</p>
                    </div>
                </div>
           </div>

           
        </div>


        
    </div>
</template>

<script>
import axios from "axios"

export default {
    name: "pokemon",
    props: {
        nameP: String,
        num: Number,
        url: String
    },
    //Computed
    data(){
        return {
            isFront: true,
            pokemon: {
                type: "",
                back: "",
                front: ""
            }
        }
    },
    created: function(){
        axios.get(this.url).then(res=>{

            this["pokemon"]["type"] = res["data"]["types"]["0"]["type"]["name"]
            this["pokemon"]["back"] = res["data"]["sprites"]["back_default"]
            this["pokemon"]["front"] = res["data"]["sprites"]["front_default"]

        }).catch(err => console.error(err))
    },
    filters: {
        bigName: (value) => {
            const arr = String(value).split("")
            arr[0] = arr[0].toUpperCase()
            
            return arr.join("")
        }
    },
    methods: {
        changeSide: function(){ this.isFront == true ? this.isFront = false : this.isFront = true },

        hover: () => { document.body.style.cursor = "pointer" },

        AfterHover: () => { document.body.style.cursor = "initial" }
    }    
}
</script>

<style scoped>

</style>