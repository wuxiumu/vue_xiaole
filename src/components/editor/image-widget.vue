<template>
  <draggable class="img_effect_file"
             :class="{active: active}"
             :x="position.x"
             :y="position.y"
             :w="position.width"
             :h="position.height"
             :active.sync="active">
    <a href="javascript:;"
       class="close"
       @click="$emit('close')">close</a>
    <slot></slot>
  </draggable>
</template>

<script>
export default {
  name: 'image-widget',
  model: {
    prop: 'position',
    event: 'change'
  },
  props: {
    position: {
      type: Object,
      default() {
        return {
          x: 0,
          y: 0,
          width: 100,
          height: 100
        }
      }
    },
    ratioWidth: {
      type: Number,
      default: 1.0
    },
    ratioHeight: {
      type: Number,
      default: 1.0
    }
  },
  data() {
    return {
      x: 100,
      y: 100,
      width: 100,
      height: 100,
      active: false
    }
  }
}
</script>
<style scoped>
.img_effect_file {
  background-color: #ccc;
  border: 1px dashed gray;
  cursor: move;
}

/* TODO
这里可以控制关闭按钮的显示隐藏
其中 .active 是在当前框为选中状态的时候才有
*/
/* .widget .close {
  display: none;
}
.widget.active:hover .close {
  display: inline-block;
} */
</style>
