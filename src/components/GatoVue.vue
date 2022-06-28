<template>
  <div class="card-img mb-3">
    <img v-show="isLoaded" v-if="this.cat" :src="this.cat" @load="this.$parent.onImgLoad" alt="gato"
         class="img-fluid rounded-3">
    <div v-show="!isLoaded" class="spinner-border text-secondary" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "GatoVue",
    props: {
      cat: String,
      isLoaded: Boolean
    }
  }
</script>

<style scoped>

</style>