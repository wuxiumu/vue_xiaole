<template>
  <editor submit-api="/api/save"
          :model="model"
          @create="create"
          @remove="remove"
          @save="save"></editor>
</template>

<script>
import Editor from './components/editor'
export default {
  name: 'background-editor',
  components: {
    Editor
  },
  data() {
    return {
      model: {}
    }
  },
  created() {
    let vm = this
    $.getJSON('/api/placeholders').then(res => {
      vm.model = res
    })
  },
  methods: {
    create({ x, y, width, height }) {
      this.model.positions.push({
        x,
        y,
        width,
        height
      })
    },
    remove(index) {
      this.model.positions.splice(index, 1, null)
    },
    save(result) {
      if (result.success) {
        location.reload(true)
      } else {
        alert(result.error)
      }
    }
  }
}
</script>
