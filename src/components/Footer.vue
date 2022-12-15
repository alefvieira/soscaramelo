<template>
  <v-footer class="cor-footer" dark padless>
      <v-card-text class=""><hr>
        Copyright ©  {{ new Date().getFullYear() }} -
        <strong>ESDEV - Todos os Direitos reservados.</strong>
        <!-- <strong>ESDEV - Todos os Direitos e Esquerdos estão reservados!</strong> -->
      </v-card-text>
  </v-footer>
</template>
<script>

</script>
<style>
  .cor-footer{
    text-align: center;
    background-color: #ffee1f !important;
    color: black !important;
  }
  a.mm{
    text-decoration: none;
    color:white !important;
    padding: 20px;
  }
  a.mm:hover{
    text-decoration: none;
    color:#00000080 !important;
  }
  .v-card__title {
    justify-content: center;
  }
</style>