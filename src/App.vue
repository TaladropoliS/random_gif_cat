<template>
  <div id="app" class="p-sm-5">

    <div class="card rounded-3">
      <div class="card-title">
        <h1 class="mt-3">Random Gift Cat</h1>
      </div>
      <div class="card-body">
        <div class="input-group mb-3">
          <span class="input-group-text" id="titulo">Título</span>
          <input v-model="titulo" type="text" class="form-control"
                 name="titulo" placeholder="Título" aria-label="Título"
                 aria-describedby="titulo" required>
        </div>
        <div class="row">
          <div class="col-sm-6">
            <select v-model="selectedFiltro" class="form-select mb-3" name="filtro" id="filtro"
                    aria-label="Default select example" required>
              <option v-for="(option, index) in optionsFiltro" :key="index" :value="option.value">
                {{ option.text }}
              </option>
            </select>
          </div>
          <div class="col-sm-6">
            <div class="row mb-3">
              <div class="col-9">
                <select v-model="selectedColor" class="form-select" name="color" id="color"
                        aria-label="Default select example" required>
                  <option v-for="(option, index) in optionsColor" :key="index" :value="option.value">
                    {{ option.text }}
                  </option>
                </select>
              </div>
              <div class="col-3 text-center">
                <div class="mx-auto my-auto" :style="backgroundColor"
                     style="border: solid 1px black; border-radius: 50%; width: 30px; height: 30px;">
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="input-group mb-3">
          <span class="input-group-text" id="tamanio">Tamaño</span>
          <input v-model.number="tamanio" type="number" class="form-control" name="tamanio" placeholder="Tamaño"
                 aria-label="Tamaño" required
                 aria-describedby="tamanio">
        </div>

        <div class="p-2 mb-2 border rounded-3">
          <i class="fa-solid fa-cat fa-3x"></i> nombre: <b>{{ titulo }}</b>, filtro: <b>{{ selectedFiltro }}</b>,
          color <b>{{ selectedColor }}</b>, tamaño: <b>{{ tamanio }}</b>
        </div>

        <button @click.prevent="llamarCat(titulo, selectedFiltro, selectedColor, tamanio)"
                type="submit"
                class="btn btn-primary">
          <i class="fas fa-search"></i>
          Buscar Cat
        </button>
      </div>

      <div class="card-img">
        <img v-show="isLoaded" v-if="this.cat" :src="this.cat" @load="onImgLoad" alt="gato"
             class="img-fluid">
        <div v-show="!isLoaded" class="spinner-border text-secondary" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>
    </div>

  </div>
</template>

<script>

  export default {
    name: 'App',
    components: {},
    data() {
      return {
        cat: '',
        titulo: '',
        tamanio: '',
        selectedColor: '',
        optionsColor: [
          {text: 'Naranjo', value: 'orange'},
          {text: 'Blanco', value: 'white'},
          {text: 'Rojo', value: 'red'},
          {text: 'Verde', value: 'green'},
          {text: 'Amarillo', value: 'yellow'},
          {text: 'Azul', value: 'blue'},
        ],
        selectedFiltro: '',
        optionsFiltro: [
          {text: 'Blur', value: 'blur'},
          {text: 'Mono', value: 'mono'},
          {text: 'Sepia', value: 'sepia'},
          {text: 'Negative', value: 'negative'},
          {text: 'Paint', value: 'paint'},
          {text: 'Pixel', value: 'pixel'},
        ],
        isLoaded: false,
      }
    },
    computed: {
      backgroundColor() {
        return {
          'background-color': this.selectedColor,
          'color': this.selectedColor
        }
      }
    },
    methods: {
      async llamarCat(ti, fi, co, ta) {
        if (this.titulo && this.selectedFiltro && this.selectedColor && this.tamanio) {
          this.isLoaded = false
          ti = this.titulo
          fi = this.selectedFiltro
          co = this.selectedColor
          ta = this.tamanio
          try {
            this.cat = await (
                `https://cataas.com/cat/gif/says/${ti}?filter=${fi}&color=${co}&size=${ta}&type=or`
            )
          } catch (error) {
            console.warn(error)
          }
        }
      },
      onImgLoad() {
        this.isLoaded = true
      }
    },
    created() {
      this.titulo = 'holi'
      this.selectedFiltro = 'sepia'
      this.selectedColor = 'orange'
      this.tamanio = '80'
      this.llamarCat('holi', 'sepia', 'orange', '80')
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.row {
  padding: 0;
  margin: 0;
}
</style>
