<!--xiolng-->
<!--CreateTenantManage-->
<!--2020/10/23-->
<!--10:02-->
<template>
  <div class="CreateTenantManage">
    <a-modal
      :visible="visible"
      :title="editId ? '编辑' : '新建'"
      @cancel="$emit('cancel')"
      @ok="$emit('create')"
    >
      <a-form :form="form" :label-col="{span: 6}" :wrapper-col="{span: 16}">
        <a-form-item label="租户名称">
          <a-input
            placeholder="请输入租户名称"
            v-decorator="[
              `tenantName`,
              {
                rules: [{required: true, message: '请输入租户名称'}]
              }
            ]"
          />
        </a-form-item>
        <a-form-item label="用户名">
          <a-input
            placeholder="请输入用户名"
            v-decorator="[
              `username`,
              {
                rules: [{required: true, message: '请输入用户名'}]
              }
            ]"
          />
        </a-form-item>
        <a-form-item label="密码">
          <a-input-password
            placeholder="请输入密码"
            v-decorator="[
              `password`,
              {
                rules: [{required: true, message: '请输入密码'}]
              }
            ]"
          />
        </a-form-item>
        <a-form-item label="真实姓名">
          <a-input
            placeholder="请输入真实姓名"
            v-decorator="[
              `realName`,
              {
                rules: [{required: true, message: '请输入真实姓名'}]
              }
            ]"
          />
        </a-form-item>
        <a-form-item label="联系电话">
          <a-input
            placeholder="请输入联系电话"
            v-decorator="[
              `phone`,
              {
                rules: [{required: true, message: '请输入联系电话'}]
              }
            ]"
          />
        </a-form-item>
        <a-form-item label="租户状态">
          <a-switch
            placeholder="请输入租户状态"
            v-decorator="[
              `tenantStatus`,
              {
                initialValue: true,
                valuePropName: 'checked'
              }
            ]"
          />
        </a-form-item>
        <a-form-item label="租户描述">
          <a-textarea
            min="2"
            max="4"
            placeholder="请输入租户描述"
            v-decorator="[
              `tenantDetail`
            ]"
          />
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>

<script>
  import { tenanteDetailApi } from '@/api/TenantManageApi'

  export default {
    name: 'CreateTenantManage',
    props: {
      visible: Boolean,
      editId: String
    },
    data () {
      return {
        form: this.$form.createForm(this, { name: 'form' }),
        shopList: [],
        poolList: [],
        showDrawer: false,
        func: null,
        selectList: []
      }
    },
    mounted () {
      this.editId && this.getDetail()
    },
    methods: {
      getDetail () {
        tenanteDetailApi({ id: this.editId }).then(res => {
          const defualtData = ['password', 'phone', 'realName', 'tenantDetail', 'tenantName', 'tenantStatus', 'username']
          if (res.data.code === '200') {
            const { data } = res.data
            Object.values(defualtData).map(v => {
              if (v === 'tenantStatus') {
                this.form.setFieldsValue({ [v]: !!data[v] })
              } else {
                this.form.setFieldsValue({ [v]: data[v] })
              }
            })
          }
        })
      },
    },
  }
</script>

<style scoped>
  .CreateTenantManage {

  }
</style>
