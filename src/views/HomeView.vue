<template>
  <div class="home">
    <h1>Home Page</h1>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Fugit esse, ipsam eum quos atque voluptates libero dicta sed veniam laborum sequi saepe quod necessitatibus in doloribus dolorem, fuga sint deserunt!</p>

    <v-btn @click="acessarGeoLocation"> acessar sua localização</v-btn>
    <h4>
      Minha localização
    </h4>

    latitude: {{geo.latitude}} <br>
    longitude: {{geo.longitude}}
  </div>
</template>

<script>
export default {
  name: "UsuarioForm",
  data: () => ({

    geo: {},
      usuario:{

      },
      valid: true,
      listaUsuario: [],
      indice: -1,
  }),
  mounted() {

  },
  methods: {
    acessarGeoLocation(){
      const log = (position)=> {
        this.geo = position.coords;
        console.log(this.geo)
      }
      navigator.geolocation.getCurrentPosition(log)
    }
  }
}
</script>

<style scoped>

.home{
  padding: 130px  2% 0 2%;
}

</style>