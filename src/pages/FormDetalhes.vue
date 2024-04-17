<template>
  <q-page padding>
    <div class="row text-align-center">
      <div class="col-6">
        <p class="label">Número do corpo</p>
        <p>{{post.numero_corpo}}</p>
      </div>
      <div class="col-6">
        <p class="label">Número de superficiários: </p>
        <p>{{post.numero_superficiarios}}</p>
      </div>
      <div class="col-6">
        <p class="label">Perímetro da área</p>
        <p>{{post.perimetro_area}}</p>
      </div>
      <div class="col-6">
        <p class="label">Situação da área</p>
        <p>{{post.situacao_area}}</p>
      </div>
      <div class="col-6">
        <p class="label">Data de abertura</p>
         <p>{{post.data_abertura}}</p>
      </div>
      <div class="col-6">
        <p class="label">Data de reabilitação</p>
         <p>{{post.data_reabilitacao}}</p>
      </div>
      <div class="col-6 my-card">
        <p class="label">Imagem da área</p>
         <img :src="post.imagem_area" :alt="post.imagem_area">
      </div>
      <div class="col-6">
        <p class="label">Critério de conclusão</p>
        <p>{{post.criterio_conclusao}}</p>
      </div>
    </div>
  </q-page>
</template>

<script>
import postServices from 'src/services/posts'
import { useRoute } from 'vue-router'
import { onMounted, ref } from 'vue'
export default {
  name: 'formDetalhes',
  setup () {
    const post = ref({})
    const { getById } = postServices()
    const route = useRoute()

    onMounted(async () => {
      if (route.params.id) {
        getPost(route.params.id)
      }
    })

    const getPost = async (id) => {
      try {
        const response = await getById(id)
        post.value = response
      } catch (error) {
        console.error(error)
      }
    }
    return { post }
  }
}
</script>

<style lang="sass" scoped>
.my-card img
  width: 100%
  max-width: 400px
.label
  font-weight: bold
</style>
