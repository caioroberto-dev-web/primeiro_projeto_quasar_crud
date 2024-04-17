<template>
  <q-page padding>
    <q-form @submit="onSubmit" class="row q-col-gutter-sm">
      <q-input
        class="col-md-6 col-xl-6 col-12"
        outlined
        v-model="form.numero_corpo"
        label="Numero do corpo"
        type="number"
        hint="Insira o número corpo"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
        class="col-md-6 col-xl-6 col-12"
        outlined
        v-model="form.numero_superficiarios"
        label="Nº de superficiários"
        type="number"
        hint="Informe o número de superficiários"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
      class="col-md-6 col-xl-6 col-12"
      outlined
      v-model="form.perimetro_area"
      label="Perímetro da área"
      type="number"
      hint="Informe o perímetro da área"
      lazy-rules
      :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
      class="col-md-6 col-xl-6 col-12"
      outlined
      v-model="form.situacao_area"
      label="Situação da área"
      hint="Informe a situação da área"
      lazy-rules
      :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
      class="col-md-6 col-xl-6 col-12"
      outlined
      v-model="form.data_abertura"
      label="Data de abertura"
      hint="Informe a data de abertura"
      type="date"
      lazy-rules
      :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
      class="col-md-6 col-xl-6 col-12"
      outlined
      v-model="form.data_reabilitacao"
      label="Data de reabilitação"
      hint="Informe a data de reabilitação"
      type="date"
      lazy-rules
      :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
      class="col-md-6 col-xl-6 col-12"
      outlined
      v-model="form.imagem_area"
      label="Imagem da área"
      hint="Insira a imagem da área"
      lazy-rules
      :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <q-input
      class="col-md-6 col-xl-6 col-12"
      outlined
      v-model="form.criterio_conclusao"
      label="Critérios de conclusão"
      hint="Informe os critérios de conclusão"
      type="textarea"
      lazy-rules
      :rules="[(val) => (val && val.length > 0) || 'Campo obrigatório!']"
      />
      <div class="col-12 q-gutter-sm">
        <q-btn class="float-right" color="primary" type="submit" icon="save"
          >Salvar</q-btn
        >
        <q-btn
          class="float-right"
          color="negative"
          type="submit"
          icon="cancel"
          :to="{ name: 'home' }"
          >Cancelar</q-btn
        >
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import postServices from 'src/services/posts'
import { useQuasar } from 'quasar'
import { useRouter, useRoute } from 'vue-router'

export default defineComponent({
  name: 'PostForm',
  setup () {
    const { post, getById, update } = postServices()
    const $q = useQuasar()
    const router = useRouter()
    const route = useRoute()
    const form = ref({
      nome: '',
      profissao: ''
    })

    onMounted(async () => {
      if (route.params.id) {
        getPost(route.params.id)
      }
    })

    const getPost = async (id) => {
      try {
        const response = await getById(id)
        form.value = response
      } catch (error) {
        console.error(error)
      }
    }
    const onSubmit = async () => {
      try {
        if (form.value.id) {
          await update(form.value)
          $q.notify({
            message: 'Atualizado com sucesso!',
            icon: 'check',
            color: 'green'
          })
          router.push({ name: 'home' })
        } else {
          await post(form.value)
          $q.notify({
            message: 'Salvo com sucesso!',
            icon: 'check',
            color: 'green'
          })
          router.push({ name: 'home' })
        }
      } catch (error) {
        console.error(error)
      }
    }
    return {
      form,
      onSubmit
    }
  }
})
</script>
