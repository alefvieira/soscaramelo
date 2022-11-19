<template>
  <div class="anime">
    <!-- <h1>Anime Page</h1> -->
    <AnimeForm />
  </div>
</template>

<script>
import AnimeForm from "@/components/AnimeForm.vue";

export default {
  name: "AnimeView",
  components: {
    AnimeForm,
  },
};
</script>
