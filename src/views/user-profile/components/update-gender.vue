<template>
  <div class="update-gender">
    <van-picker title="标题"
                show-toolbar
                :columns="columns"
                :default-index="localGender"
                @cancel="$emit('close')"
                @confirm="onConfirm"
                @change="onPickerChange" />
  </div>
</template>

<script>
import { updateUserProfile } from '@/api/user'
export default {
  name: 'UpdateGender',
  components: {

  },
  props: {
    value: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      columns: ['男', '女'],
      localGender: this.value
    };
  },
  computed: {

  },
  watch: {

  },
  methods: {
    async onConfirm () {
      this.$toast.loading({
        message: '保存中...',
        forbidClick: true,
        duration: 0
      })
      try {
        const localGender = this.localGender
        await updateUserProfile({
          gender: localGender
        })
        this.$emit('input', localGender)
        this.$emit('close')
        this.$toast.success('更新成功')
      } catch (error) {
        this.$toast.fail('更新失败')
      }
    },
    onPickerChange (picker, value, index) {
      this.localGender = index
      console.log(this.localGender);
    }
  },
  created () {

  },
  mounted () {

  },
}
</script>

<style lang='less' scoped>
</style>