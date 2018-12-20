<template>
  <div class="author">
    <h3>Author</h3>
    <div class="row">
      <div class="col-sm-4">
        <p class="text-right">
          <button type="button" class="btn btn-primary btn-sm" @click="create">Tambah</button>
        </p>
        <div v-if="items.length" class="list-group">
          <div v-for="(item, i) in items" 
              :key="i" 
              class="list-group-item">
                <a href="#" @click.prevent="edit(item)">{{ item.name }}</a>
                <a href="#" @click.prevent="remove(item, i)" class="text-danger float-right">
                  <small>Hapus</small>
                </a>
          </div>
        </div>
        <div v-else class="text-center text-muted"><em>-- Tidak ada data --</em></div>
      </div>
      <div class="col-sm-4">
        <!-- Form Create -->
        <author-create-edit 
          v-show="isCreate" 
          :item="item" 
          @submit="store" 
          @batal="reset" />

        <!-- Form Edit -->
        <author-create-edit 
          v-show="isEdit" 
          :item="item" 
          @submit="update" 
          @batal="reset" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from '@/services/Axios';
// component
import AuthorCreateEdit from '@/components/author/CreateEdit.vue';

export default {
  name: 'author-index',
  components: { AuthorCreateEdit },
  data(){
    return {
      items: [],
      item: null,
      isCreate: false,
      isEdit: false,
    }
  },
  // fetch data lebih baik menggunakan fungsi 'beforeRouteEnter' dari vue-router
  
  // beforeRouteEnter(to, from, next){
  //   axios.get('author').then(res => {
  //     const data = res.data.data
  //     if(data){
  //       next(vm => vm.items = data)
  //     }
  //   })
  //   .catch(error => {
  //     next(false)
  //   })
  // },

  created(){
    this.get() // remove this if you used 'beforeRouteEnter'
  },
  methods: {
    get(){
      axios.get('author').then(res => {
        this.items = res.data.data
      }).catch(error => {
        console.log(error)
      })
    },
    create(){
      this.item = {name: null}
      this.isCreate = true
      this.isEdit = false
    },
    store(item){
      axios.post('author', item).then(res => {
        this.get()
        this.reset()
        setTimeout(()=>{
          alert('Berhasil create')
        }, 500)
      })
      .catch(error => {
        console.log(error)
      })
    },
    edit(item){
      this.item = item
      this.isEdit = true
      this.isCreate = false
    },
    update(item){
      axios.put('author/'+item.id, item).then(res => {
        this.reset()
        setTimeout(()=>{
          alert(`Berhasil update '${item.name}'`)
        }, 500)
      })
      .catch(error => {
        console.log(error)
      })
    },
    remove(item, i){
      if(!confirm(`Yakin hapus author '${item.name}' ?`)) return false

      axios.delete('author/'+ item.id).then(res => {
        alert(`berhasil Hapus '${item.name}'`)
        // remove item
        this.items.splice(i, 1)
      })
      .catch(error => {
        console.log(error)
      })
    },
    reset(){
      this.item = null
      this.isEdit = false
      this.isCreate = false
    }
  }
}
</script>
