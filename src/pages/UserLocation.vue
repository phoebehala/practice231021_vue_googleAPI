<template>
  <section  class="ui two column centered grid">
    <div class="column">
        <form action="" class="ui segment large form">
            <div class="ui message red"> </div>
            <div class="ui segment"> 
                <div class="field">
                    <div class="ui right icon input large">
                        <input type="text" placeholder="Enter">
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
export default{
    methods: {
        locatorButtonPressed(){
            console.log('hi')
            if(navigator.geolocation){
                navigator.geolocation.getCurrentPosition(
                    position =>{
                        console.log(position.coords.latitude)
                        console.log(position.coords.longitude)
                    },
                    error =>{
                        console.log(error.message)
                    }
                )  
            }else{
                console.log("Your browser does not support geolocation API")
            }
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
