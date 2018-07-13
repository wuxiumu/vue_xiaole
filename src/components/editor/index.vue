<template>
 
  <div class="editor-container">
    <img class="img_effect_bg"
         :src="model.background"
         alt="background-image" />
    <template v-for="(position, index) in model.positions">
      <widget :key="index"
              v-if="position"
              :position="position"
              :ratio-width="ratioWidth"
              :ratio-height="ratioHeight"
              @close="remove(index)">
      </widget>
    </template>
    <menu-list v-show="showMenu"
               :x="menuX"
               :y="menuY"
               @create="create"
               @save="save"></menu-list>
  </div>
</template>

<script>
import mixins from '../mixins'
import Widget from './image-widget'
import MenuList from './menu'
import ImageWidget from 'vue-draggable-resizable'
Vue.component('draggable', ImageWidget)

export default {
  name: 'background-editor',
  mixins: [mixins],
  components: {
    Widget,
    MenuList
  },
  props: {
    model: {
      type: Object,
      required: true
    },
    submitApi: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      showMenu: false,
      bgX: 0,
      bgY: 0,
      menuX: 0,
      menuY: 0,
      originalAvatars: [],
      avatars: [],
      ratio: 1.0,
      ratioWidth: 1.0,
      ratioHeight: 1.0
    }
  },
  methods: {
    create(e) {
      this.$emit('create', {
        x: e.x,
        y: e.y,
        width: 150,
        height: 150
      })
    },
    remove(index) {
      this.$emit('remove', index)
    },
    save() {
      let vm = this
      const { background, positions } = vm.model
      const { ratioWidth, ratioHeight, ratio } = vm
      alert('xxx' + ratioWidth + '---' + ratioHeight)
      let pos = []
      $('.img_effect_file').each(function() {
        let $el = $(this)
        const { top, left } = $el.position()
        pos.push({
          x: left / ratioWidth,
          y: top / ratioHeight,
          width: $el.width() / ratioWidth,
          height: $el.height() / ratioHeight
        })
      })

      let result = {
        positions: pos,
        background,
        ratio
      }
      console.log(result)
      $.ajax({
        url: this.submitApi,
        contentType: 'application/json',
        data: JSON.stringify(result),
        dataType: 'json',
        type: 'POST'
      }).then(
        res => {
          vm.$emit('save', {
            success: true
          })
        },
        error => {
          vm.$emit('save', {
            success: false,
            error: error
          })
        }
      )
    }
  },
  mounted() {
    let vm = this
    document.oncontextmenu = function() {
      return false
    }
    document.body.onmouseup = function(e) {
      if (e.button === 2) {
        console.log([vm.bgY, vm.bgX])
        vm.menuX = e.pageX - vm.bgX
        vm.menuY = e.pageY - vm.bgY
        vm.showMenu = true
      } else {
        vm.showMenu = false
      }
    }
  }
}
</script>

<style scoped>
/* 
.img_effect_bg {
  width: 100%;
  z-index: -1;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
} */
.editor-container {
  position: relative;
}

.img_effect {
  margin: 0 10px;
}

.img_effect .img_box {
  position: relative;
  width: 100%;
}

.img_effect .img_box .img_effect_bg {
  display: block;
  width: 100%;
}

.img_effect .img_box .img_effect_file {
  position: absolute;
  overflow: hidden;
  border: 1px solid transparent;
}

.img_effect .img_box .img_effect_file.on {
  border: 1px solid #fb5d5a;
}

.img_effect .img_box .img_effect_file img {
  position: absolute;
}
</style>