<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="" prop="name">
        <el-input v-model="dataForm.name" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="idCard">
        <el-input v-model="dataForm.idCard" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="phone">
        <el-input v-model="dataForm.phone" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="communityCode">
        <el-input v-model="dataForm.communityCode" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="residentialId">
        <el-input v-model="dataForm.residentialId" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="address">
        <el-input v-model="dataForm.address" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="avatarUrl">
        <el-input v-model="dataForm.avatarUrl" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="status">
        <el-input v-model="dataForm.status" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="createTime">
        <el-input v-model="dataForm.createTime" placeholder=""></el-input>
      </el-form-item>
      <el-form-item label="" prop="modifyTime">
        <el-input v-model="dataForm.modifyTime" placeholder=""></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          name: '',
          idCard: '',
          phone: '',
          communityCode: '',
          residentialId: '',
          address: '',
          avatarUrl: '',
          status: '',
          createTime: '',
          modifyTime: ''
        },
        dataRule: {
          name: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          idCard: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          communityCode: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          residentialId: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          avatarUrl: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          modifyTime: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/sys/landlordrentapply/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.landlordRentApply.name
                this.dataForm.idCard = data.landlordRentApply.idCard
                this.dataForm.phone = data.landlordRentApply.phone
                this.dataForm.communityCode = data.landlordRentApply.communityCode
                this.dataForm.residentialId = data.landlordRentApply.residentialId
                this.dataForm.address = data.landlordRentApply.address
                this.dataForm.avatarUrl = data.landlordRentApply.avatarUrl
                this.dataForm.status = data.landlordRentApply.status
                this.dataForm.createTime = data.landlordRentApply.createTime
                this.dataForm.modifyTime = data.landlordRentApply.modifyTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/sys/landlordrentapply/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'idCard': this.dataForm.idCard,
                'phone': this.dataForm.phone,
                'communityCode': this.dataForm.communityCode,
                'residentialId': this.dataForm.residentialId,
                'address': this.dataForm.address,
                'avatarUrl': this.dataForm.avatarUrl,
                'status': this.dataForm.status,
                'createTime': this.dataForm.createTime,
                'modifyTime': this.dataForm.modifyTime
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
