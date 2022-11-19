<template>
  <div class="plataforma">
    <h1>Plataforma Page</h1>
    <PlataformaForm/>
  </div>
</template>

<script>
  import PlataformaForm from "../components/PlataformaForm.vue"

  export default {
    name: "PlataformaView",
    components:{
      PlataformaForm,
    }
  }
</script>
