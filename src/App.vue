<template>
  <div id="app" class="p-sm-5">

    <div class="card rounded-3">
      <div class="card-title">
        <h1 class="mt-3">Random Gift Cat</h1>
      </div>
      <div class="card-body">
        <div class="row">
          <div class="col-lg-3">
            <div class="input-group mb-3">
              <span class="input-group-text" id="titulo">Título</span>
              <input v-model="titulo" type="text" class="form-control"
                     name="titulo" placeholder="Título" aria-label="Título"
                     aria-describedby="titulo" required>
            </div>
          </div>
          <div class="col-lg-6">
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
                    <div class="mx-auto my-auto" :style="background"
                         style="border: solid 1px black; border-radius: 50%; width: 30px; height: 30px;">
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-3">
            <div class="input-group mb-3">
              <span class="input-group-text" id="tamanio">Tamaño</span>
              <input v-model.number="tamanio" type="number" class="form-control" name="tamanio" placeholder="Tamaño"
                     aria-label="Tamaño" required
                     aria-describedby="tamanio">
            </div>
          </div>
        </div>
        <div class="p-2 mb-2 border rounded-3">
          <i class="fa-solid fa-cat fa-3x"></i>
          nombre: <b>{{ titulo }}</b>,
          filtro: <b>{{ selectedFiltro }}</b>,
          color <span class="fw-bold" :style="color"><b>{{ selectedColor }}</b></span>,
          tamaño: <b>{{ tamanio }}</b>
        </div>

        <button @click.prevent="llamarCat(titulo, selectedFiltro, selectedColor, tamanio)"
                type="submit"
                class="btn btn-primary">
          <i class="fas fa-search"></i>
          Buscar Cat
        </button>
      </div>


      <GatoVue :isLoaded="isLoaded"
               :cat="this.cat"/>

    </div>

  </div>
</template>

<script>
  import GatoVue from "@/components/GatoVue";

  export default {
    name: 'App',
    components: {
      GatoVue
    },
    data() {
      return {
        cat: '',
        titulo: '',
        tamanio: '',
        selectedColor: '',
        optionsColor: [
          {text: 'Naranjo', value: 'darkorange'},
          {text: 'Indigo', value: 'indigo'},
          {text: 'Rojo', value: 'red'},
          {text: 'Verde', value: 'green'},
          {text: 'Café', value: 'saddlebrown'},
          {text: 'Azul', value: 'royalblue'},
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
      background() {
        return {
          'background-color': this.selectedColor,
        }
      },
      color() {
        return {
          'background-color': this.selectedColor,
          'color': 'white',
          'padding': '5px',
          'border-radius': '15px',
        }
      },
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
      },
    },
    created() {
      this.titulo = 'rex'
      this.selectedFiltro = 'paint'
      this.selectedColor = 'red'
      this.tamanio = '100'
      this.llamarCat(this.titulo, this.selectedFiltro, this.selectedColor, this.tamanio)
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
