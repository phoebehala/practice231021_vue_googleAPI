<template>
  <section  class="ui two column centered grid">
    <div class="column">
        <form action="" class="ui segment large form">
            <div class="ui message red"> </div>
            <div class="ui segment"> 
                <div class="field">
                    <div class="ui right icon input large">
                        <input type="text" placeholder="Enter" v-model="address">
                        <button class="ui button" @click="locatorButtonPressed">Go</button>
                    </div>
                </div>
                <button class="ui button">Go</button>
            </div>
        </form>
    </div>
  </section>
</template>

<script>
import axios from 'axios'
export default{

    // instant property
    data(){
        return{
            address:""
        }
    },

    methods: {
        locatorButtonPressed(){
            console.log('hi')
            if(navigator.geolocation){
                navigator.geolocation.getCurrentPosition(
                    position =>{
                        console.log(position.coords.latitude)
                        console.log(position.coords.longitude)
                        this.getAddressFrom(position.coords.latitude, position.coords.longitude)
                    },
                    error =>{
                        console.log(error.message)
                    }
                )  
            }else{
                console.log("Your browser does not support geolocation API")
            }
        },

        getAddressFrom(lat, long){
            axios.get("https://maps.googleapis.com/maps/api/geocode/json?latlng="+lat+","+long+"&key=AIzaSyBnPzzRlPOXICxku3udeR_xMModID4ir8w")
            .then(res =>{
                if(res.data.error_message){
                    console.log(res.data.error_message);
                }else{
                    //console.log(res.data.results[0].formatted_address);
                    this.address = res.data.results[0].formatted_address
                }
            })
            .catch(err=>{
                console.log(err.message);
            })
        }
    }
}
</script>

<style>
.ui.button{
    background-color: #ff5e5f;
    color: #fff;
} 
</style>
