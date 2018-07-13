<template>
  <calendar-maker submit-api="/api/combine"
                  :model="model"
                  @save="save"></calendar-maker>
</template>

<script>
import CalendarMaker from './components/app'
export default {
  name: 'app',
  components: {
    CalendarMaker
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
    save(res) {
      if (res.success && res.url) {
        // 保存成功时服务器返回url，客户端可以根据URL做需要的操作，这里是打开一个页面查看保存完的图片。
        let link = document.createElement('a')
        link.href = res.url
        link.target = '_blank'
        link.click()
      } else if (!res.success && res.error) {
        // 有错误消息时放在error字段
        alert(res.error)
      } else {
        console.log(['saved', res])
      }
    }
  }
}
</script>
