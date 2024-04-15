<template>
  <q-page padding>
    <div class="q-pa-md">
      <q-table title="Artigo" :rows="posts" :columns="columns" row-key="name">
        <template v-slot:top>
          <span class="text-h5">Artigo</span> <q-space/>
          <q-btn color="primary" label="Novo" :to="{name: 'formPost'}"/>
        </template>
        <template v-slot:body-cell-actions="props">
          <q-td :props="props" class="q-gutter-sm">
            <q-btn icon="edit" color="primary" dense size="sm" @click="handleEditPost(props.row.id)"></q-btn>
            <q-btn
              icon="delete"
              color="negative"
              dense
              size="sm"
              @click="handleDeletePost(props.row.id)"
            />
          </q-td>
        </template>
      </q-table>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue'
import postServices from 'src/services/posts'
import { useQuasar } from 'quasar'
import { useRouter } from 'vue-router'

export default defineComponent({
  name: 'IndexPage',
  setup () {
    const posts = ref([])
    const $q = useQuasar()
    const { list, remove } = postServices()
    const router = useRouter()
    const columns = [
      { name: 'id', field: 'id', label: 'Id', sortable: true },
      { name: 'nome', field: 'nome', label: 'Nome', sortable: true },
      { name: 'profissao', field: 'profissao', label: 'Profissão', sortable: true },
      { name: 'actions', field: 'actions', label: 'Ações' }
    ]
    onMounted(() => {
      getPosts()
    })
    const getPosts = async () => {
      try {
        const { data } = await list()
        posts.value = data
      } catch (error) {
        console.error(error)
      }
    }

    const handleDeletePost = async (id) => {
      try {
        $q.dialog({
          title: 'Deseja realmente deletar ?',
          message: 'item com id: ' + id,
          cancel: true,
          persistent: true
        }).onOk(async () => {
          await remove(id)
          $q.notify({ message: 'Deletado com sucesso!', color: 'green', icon: 'check' })
          await getPosts()
        })
      } catch (error) {
        $q.notify({ message: 'Houve um erro ao tentar deletar um item da tabela!', color: 'red', icon: 'announcement' })
      }
    }

    const handleEditPost = async (id) => {
      router.push({ name: 'formPost', params: { id } })
    }
    return {
      posts,
      columns,
      handleDeletePost,
      handleEditPost
    }
  }
})
</script>
