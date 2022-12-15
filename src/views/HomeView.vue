<template>
  <div class="home">
    <h1>SOS CARAMELO</h1>
    <p>O SOS CARAMELO, o Tinder do seu cãozinho/pet, para adoção ou doação.</p>

    <v-btn style="margin:10px;" to="/adotar"> Cadastrar Pet para adoção</v-btn>
    <v-btn style="margin:10px;" to="/adotar2"> Ver animais para adoção</v-btn>
    <v-img class="caramelo-safado"
                            height="150"
                            width="500"
                            src="https://c.tenor.com/EjnHPIL08yUAAAAM/cachorro-caramelo.gif"
                        >
                        </v-img>

                        <br>
                        <br>
                        <ul>
                          <li v-for="objUser in geoLocation" :key="objUser">
                            {{objUser}}
                          </li>
                        </ul>
                        {{geoLocation.city}}
    
  </div>
</template>

<script>
export default {
  name: "HomeView",
  data: () => ({
    loading: false,
    erro: "",
    position: {lat: 0, lon: 0},
    geoLocation: {}
  
  }),

  mounted() {
    this.buscaGeoLocalizacao()
  },
  methods: {
    buscaGeoLocalizacao(){
      // if (navigator.geolocation){
        navigator.geolocation.getCurrentPosition(this.showPosition);
      // }
      // else{
      //   this.localidade = "O seu navegador não suporta Geolocalização.";
      // }
    },
 

    async showPosition(position){
      this.position.lat = position.coords.latitude;
      this.position.lon = position.coords.longitude;


      const req = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${this.position.lat}&lon=${this.position.lon}`);
      const data = await req.json();
      this.geoLocation = data.address
    },
  }

};
</script>

<style scoped>

.home{
  padding: 130px  15% 0 15%;
  background-color: #ffee1f;
}
.caramelo-safado{
  padding: 140px  140px 140px 140px;
}
</style>