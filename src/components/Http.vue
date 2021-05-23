<template lang="html">
  <section class="src-componentes-http">
    <div class="jumbotron">
      <h2>Ejercicio 2 </h2>
      <hr>

      <button class="btn btn-info my-3 mr-3" @click="getInfoXML()">Pedir XMLHttpRequest</button>    
      <button class="btn btn-primary my-3 mr-3" @click="getInfoFetch()">Pedir FETCH</button>    
      <button class="btn btn-warning my-3 mr-3" @click="getInfoAxios()">Pedir AXIOS</button>    

      <div v-if="usuarios.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th v-for="(col,index) in getCols" :key="index">{{col}}</th>  
          </tr>

          <tr v-for="(info,index) in usuarios" :key="index">
            <td v-for="(col,index) in getCols" :key="index">{{info[col]}}</td>
          </tr>
        </table>
      </div>


    </div>
  </section>
  

</template>

<script lang="js">

  export default  {
    name: 'src-components-navbar',
    props: [],
    mounted () {

    },
    data () {
      return {
        url:'https://60aac34c66f1d000177732f0.mockapi.io/usuarios',
        usuarios : []
      }
    },
    methods: {

      /* XMLHttpRequest*/
      getInfoXML(){
        let url = new XMLHttpRequest
        url.open('get',this.url)
        url.addEventListener('load', () => {
          if(url.status == 200) {
            let respuesta = JSON.parse(url.response)
            console.log('XMLHttpRequest',respuesta)
            this.usuarios = respuesta
          }
          else {
            console.error(`Error en GET -> status: ${url.status}`)
          }
        })
        url.addEventListener('error', e => {
            console.error(`Error XMLHttpRequest ->`, e)
        })
        url.send()

      },

      /*fetch*/ 
      getInfoFetch(){
        fetch(this.url)
          .then(datos => datos.json())
          .then(respuesta => {
            console.log('FETCH',respuesta)
            this.usuarios = respuesta
          })
          .catch(error => console.error(error))

      },
      
      /*axios*/
      getInfoAxios(){
        this.axios(this.url)
          .then(respuesta => {
            console.log('AXIOS', respuesta.data)
            this.usuarios = respuesta.data
          })
          .catch(error => console.error(error))

      }
    },
    computed: {
      getCols() {
        return Object.keys(this.usuarios[0])
      }
    }
}


</script>

<style scoped lang="css">
  .src-components-navbar {

  }
</style>
