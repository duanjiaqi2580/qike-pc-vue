<template>
  <el-select :value="value" @change="fn" clearable placeholder="请选择">
    <el-option :key="item.id" :label="item.name" :value="item.id" v-for="item in channelOptions"></el-option>
  </el-select>
</template>

<script>
export default {
  name: 'my-channel',
  props: ['value'],
  data () {
    return {
      // value: null,
      channelOptions: []
    }
  },
  created () {
    this.getChannelOptions()
  },
  methods: {
    // 频道改变函数
    fn (channelId) {
      // 清空值是'' 改成null
      if (channelId === '') channelId = null
      // 把ID提交给父组件
      this.$emit('input', channelId)
    },
    // 获取频道选项数据
    async getChannelOptions () {
      // 获取数据
      const {
        data: { data }
      } = await this.$http.get('channels')
      // 赋值 channelOptions
      this.channelOptions = data.channels
    }
  }
}
</script>

<style scoped lang='less'></style>
