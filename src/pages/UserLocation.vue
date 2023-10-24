<template>
    <div>
        <section  class="ui two column centered grid" style="position:relative;z-index:1">
          <div class="column">
              <form action="" class="ui segment large form">
                  <div class="ui message red"> </div>
                  <div class="ui segment"> 
                      <div class="field">
                          <div class="ui right icon input large">
                                <!-- <input  type="querryText" placeholder="Enter"
                                      v-model="querryText"
                                      @focus="clearInput"  
                                      @keydown.enter="searchPlaces"
                                      @input="searchPlaces"
                                      ref="autocomplete">
                               <button class="ui button" @click="searchPlaces">Go</button> -->
                          </div>
                      </div>
                      <div class="field">
                          <div class="ui right icon input large">
                            <input  type="text" placeholder="Enter" 
                                      @focus="clearInput"  
                                      ref="autocomplete">
                              <button class="ui button" @click="locatorButtonPressed">My current place</button>
                          </div>
                      </div>
                      
                  </div>
              </form>
          </div>
        </section>
        <section id="map">
      
        </section>

    </div>

</template>

<script>
import axios from 'axios'
export default{

    // instant property
    data(){
        return{
            address:"",
            //querryText:"",
        }
    },
    
    mounted() {
      let autocomplete = new google.maps.places.Autocomplete(
        this.$refs["autocomplete"],
        {
          bounds: new google.maps.LatLngBounds(
            new google.maps.LatLng(45.4215296, -75.6971931)
          ),
        }
      );
  
      autocomplete.addListener("place_changed", () => {
        let place = autocomplete.getPlace();
  
        //console.log(place);
        this.showUserLocationOnTheMap(
            place.geometry.location.lat(),
            place.geometry.location.lng()
        );
      });
    },
  

    methods: {
        clearInput() {
            this.address = '';
        },
        /*
        searchPlaces(){
          
            axios.get("https://maps.googleapis.com/maps/api/place/textsearch/json?query="+this.querryText+"&key="+process.env.VITE_APP_GOOGLE_MAP_API_KEY)
            .then(res =>{
                if(res.data.error_message){
                    console.log(res.data.error_message);
                }else{
                    console.log(res.data);
                    
                }
            })
            .catch(err=>{
                console.log(err.message);
            })
            
            
        },
        */
        locatorButtonPressed(){
            console.log('hi')
            if(navigator.geolocation){
                navigator.geolocation.getCurrentPosition(
                    position =>{
                        console.log(position.coords.latitude)
                        console.log(position.coords.longitude)
                        this.getAddressFrom(position.coords.latitude, position.coords.longitude)
                        this.showUserLocationOnTheMap(position.coords.latitude, position.coords.longitude)
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
            //console.log(process.env.VUE_APP_GOOGLE_MAP_API_KEY);  //???
            axios.get("https://maps.googleapis.com/maps/api/geocode/json?latlng="+lat+","+long+"&key="+process.env.VUE_APP_GOOGLE_MAP_API_KEY)
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
        },
        showUserLocationOnTheMap(lat, long){
            // create a map object
            var map = new google.maps.Map(document.getElementById('map'), {
                zoom: 15,
                center: new google.maps.LatLng(lat, long),
                mapTypeId: google.maps.MapTypeId.ROADMAP,
            });

            // add marker
            new google.maps.Marker({
            position: new google.maps.LatLng(lat, long),
            map: map,
            });
        }
    }
}
</script>

<style>
  .ui.button,
  .dot.circle.icon {
    background-color: #ff5a5f;
    color: white;
  }
  
  .pac-icon {
    display: none;
  }
  
  .pac-item {
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
  }
  
  .pac-item:hover {
    background-color: #ececec;
  }
  
  .pac-item-query {
    font-size: 16px;
  }
  
#map {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background-color: red;
  }
</style>
