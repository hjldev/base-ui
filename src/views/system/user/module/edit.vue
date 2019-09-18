<template>
  <div>
    <el-button size="mini" type="primary" icon="el-icon-edit" @click="to"/>
    <eForm ref="form" :sup_this="sup_this" :is-add="false" :dicts="dicts"/>
  </div>
</template>
<script>
import eForm from './form'
import { getRolesByUserId } from '@/api/role'
export default {
  components: { eForm },
  props: {
    data: {
      type: Object,
      required: true
    },
    // index.vue 的this 可用于刷新数据
    sup_this: {
      type: Object,
      required: true
    },
    dicts: {
      type: Array,
      required: true
    }
  },
  methods: {
    to() {
      const _this = this.$refs.form
      _this.getRoles()
      _this.getDepts()
      _this.getRoleLevel()
      _this.roleIds = []
      _this.form = { id: this.data.id, username: this.data.username, phone: this.data.phone, email: this.data.email, enabled: this.data.enabled.toString(), deptId: this.data.deptId, jobId: this.data.jobId }
      getRolesByUserId(this.data.id).then(role => {
        role.forEach((item, index, arr) => {
          _this.roleIds.push(item.id)
        })
      })
      _this.deptId = this.data.deptId
      _this.jobId = this.data.jobId
      _this.getJobs(_this.deptId)
      _this.dialog = true
    }
  }
}
</script>

<style scoped>
  div{display: inline-block;margin-right: 3px;}
</style>
