<template>
  <div class="author-edit" v-if="item">
    <div class="card">
      <div class="card-body">
        <legend>{{ item.name }}</legend>
        <form @submit.prevent="submit" class="form">
          <div class="form-group">
            <label>Nama:</label>
            <input v-model="item.name" type="text" class="form-control">
          </div>
          <button class="btn btn-success btn-block" type="submit">
            Simpan
          </button>
        </form>
      </div>
      <div class="card-footer">
        <button class="btn btn-default" @click="$emit('batal')">
          Batal
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'author-edit',
  props: {
    item: {
      default: () => null
    }
  },
  methods: {
    submit(){
      this.$emit('submit', this.item)
    }
  }
}
</script>

