<template>
  <div class="usuario">
    <h1>usuario Page</h1>
    <UsuarioForm/>
  </div>
</template>

<script>
  import UsuarioForm from "../components/UsuarioForm.vue"


  export default {
    name: "PlataformaView",
    components:{
    UsuarioForm,
}
  }
</script>
