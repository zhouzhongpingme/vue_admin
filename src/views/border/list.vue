<template>
	<section class="app-container">
		<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
			<el-form :inline="true" :model="filters" @submit.native.prevent>
				<el-form-item>
					<el-input v-model="filters.name" placeholder="姓名"></el-input>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" v-on:click="getUsers">查询</el-button>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" @click="handleAdd">新增</el-button>
				</el-form-item>
			</el-form>
		</el-col>

		<!--列表-->
		<el-table :data="users" highlight-current-row @selection-change="selsChange" style="width: 100%;">
			<el-table-column type="selection" width="55">
			</el-table-column>
			<el-table-column type="index" width="60">
			</el-table-column>
			<el-table-column prop="name" label="姓名" width="120">
			</el-table-column>
			<el-table-column prop="sex" label="性别" width="120" :formatter="formatSex">
			</el-table-column>
			<el-table-column prop="age" label="年龄" width="120">
			</el-table-column>
			<el-table-column prop="birth" label="生日" width="120">
			</el-table-column>
			<el-table-column prop="addr" label="地址" min-width="160">
			</el-table-column>
			<el-table-column label="操作" width="150">
				<template slot-scope="scope">
          <!--
          <el-button size="small" type="success">启用</el-button>
          <el-button size="small" type="info">禁用</el-button>
          -->
					<el-button size="small"  type="primary" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
					<el-button type="danger" size="small" @click="handleDel(scope.$index, scope.row)">删除</el-button>
				</template>
			</el-table-column>
		</el-table>

		<!--工具条-->
		<el-col :span="24" class="toolbar">
			<el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button>
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="20" :total="total" style="float:right;">
			</el-pagination>
		</el-col>

		<!--编辑界面-->
		<el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible" :close-on-click-modal="false">
			<el-form :model="editForm" label-width="80px" :rules="editFormRules" ref="editForm">
        <el-row>
            <el-col :span="12">
                <el-form-item label="订单编码" prop="number">
                    <el-input v-model="editForm.number" auto-complete="off"></el-input>
                </el-form-item>
            </el-col>
            <el-col :span="12">
                <el-form-item label="订单类型">
                <el-select v-model="editForm.type" placeholder="请选择">
                    <el-option key="bbk" label="采购" value="bbk"></el-option>
                    <el-option key="xtc" label="续约" value="xtc"></el-option>
                    <el-option key="bbk" label="升级" value="bbk"></el-option>
                </el-select>
                </el-form-item>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="12">
                <el-form-item label="应收金额">
                    <el-input-number v-model="editForm.payamount"></el-input-number>
                </el-form-item>
            </el-col>
            <el-col :span="12">
                <el-form-item label="折扣金额">
                    <el-input-number v-model="editForm.discount"></el-input-number>
                </el-form-item>
            </el-col>
        </el-row>

        <el-row>

            <el-col :span="12">
                <el-form-item label="实收金额">
                    <el-input-number v-model="editForm.actamount"></el-input-number>
                </el-form-item>
            </el-col>

            <el-col :span="12">
                <el-form-item label="待收金额">
                    <el-input-number v-model="editForm.debtamount"></el-input-number>
                </el-form-item>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="12">
                <el-form-item label="支付方式">
                <el-select v-model="editForm.payway" placeholder="请选择">
                        <el-option key="bbk" label="现金" value="bbk"></el-option>
                        <el-option key="xtc" label="支付宝" value="xtc"></el-option>
                        <el-option key="wechat" label="微信" value="1"></el-option>
                        <el-option key="chinabank" label="中国银行" value="2"></el-option>
                        <el-option key="chiangosahng" label="工商银行" value="3"></el-option>
                        <el-option key="nongye" label="农业银行" value="4"></el-option>
                        <el-option key="constract" label="建设银行" value="5"></el-option>
                        <el-option key="jiaotong" label="交通银行" value="6"></el-option>
                        <el-option key="other" label="其他" value="7"></el-option>
                    </el-select>
                </el-form-item>
            </el-col>

            <el-col :span="12">
                <el-form-item label="终端数量">
                    <el-input-number v-model="editForm.terminalnumber"></el-input-number>
                </el-form-item>
            </el-col>
        </el-row>


        <el-row>
            <el-col :span="12">
                <el-form-item label="订单状态">
                <el-select v-model="editForm.status" placeholder="请选择">
                        <el-option key="bbk" label="保存" value="bbk"></el-option>
                        <el-option key="xtc" label="待支付" value="xtc"></el-option>
                        <el-option key="imoo" label="已支付" value="imoo"></el-option>
                        <el-option key="imoo" label="部分支付" value="imoo"></el-option>
                        <el-option key="imoo" label="作废" value="imoo"></el-option>
                    </el-select>
                </el-form-item>
            </el-col>

            <el-col :span="12">
                <el-form-item label="终端数量">
                    <el-input-number v-model="editForm.terminalnumber"></el-input-number>
                </el-form-item>
            </el-col>
        </el-row>



    </el-form>
			<div slot="footer" class="dialog-footer">
			 <el-button @click.native="dialogFormVisible=false">取消</el-button>
			  <el-button v-if="dialogStatus=='create'" type="primary" @click="createData">添加</el-button>
              <el-button v-else type="primary" @click="updateData">修改</el-button>
			</div>
		</el-dialog>
	</section>
</template>

<script>
import util from '@/utils/table.js'
import {
  getUserListPage,
  removeUser,
  batchRemoveUser,
  editUser,
  addUser
} from '@/api/userTable'

export default {
  data() {
    return {
      dialogStatus: '',
      textMap: {
        update: '编辑',
        create: '新增'
      },
      dialogFormVisible: false,
      filters: {
        name: ''
      },
      editForm: {
            id:'' ,
            number: '',
            name: '',
            type: '',
            price: undefined,
            payamount:undefined,
            actamount:undefined,
            discount:undefined,
            debtamount:undefined,
            terminalnumber:undefined,
            payway:'',
            status:'',
            remark: '',
            createUser: '',
            createTime: '',
            lastUpdateUser:'',
            lastUpdateTime:''
          },
      users: [],
      total: 0,
      page: 1,
      sels: [], // 列表选中列
      editFormRules: {
        name: [{ required: true, message: '请输入姓名', trigger: 'blur' }],
        number:[{ required: true, message: '请输入编码', trigger: 'blur' }]
      },
      // 编辑界面数据
      editForm: {
        id: '0',
        name: '',
        sex: 1,
        age: 0,
        birth: '',
        addr: ''
      },

      addFormVisible: false, // 新增界面是否显示
      addFormRules: {
        name: [{ required: true, message: '请输入姓名', trigger: 'blur' }]
      }
    }
  },
  methods: {
    // 性别显示转换
    formatSex: function(row, column) {
      return row.sex === 1 ? '男' : row.sex === 0 ? '女' : '未知'
    },
    handleCurrentChange(val) {
      this.page = val
      this.getUsers()
    },
    // 获取用户列表
    getUsers() {
      const para = {
        page: this.page,
        name: this.filters.name
      }
      getUserListPage(para).then(res => {
        this.total = res.data.total
        this.users = res.data.users
      })
    },
    // 删除
    handleDel(index, row) {
      this.$confirm('确认删除该记录吗?', '提示', {
        type: 'warning'
      })
        .then(() => {
          const para = { id: row.id }
          removeUser(para).then(res => {
            this.$message({
              message: '删除成功',
              type: 'success'
            })
            this.getUsers()
          })
        })
        .catch(() => {})
    },
    // 显示编辑界面
    handleEdit(index, row) {
      this.dialogStatus = 'update'
      this.dialogFormVisible = true
      this.editForm = Object.assign({}, row)
    },
    // 显示新增界面
    handleAdd() {
      this.dialogStatus = 'create'
      this.dialogFormVisible = true
      this.editForm = {
        id: '0',
        name: '',
        sex: 1,
        age: 0,
        birth: '',
        addr: ''
      }
    },
    // 编辑
    updateData() {
      this.$refs.editForm.validate(valid => {
        if (valid) {
          this.$confirm('确认提交吗？', '提示', {})
            .then(() => {
              const para = Object.assign({}, this.editForm)
              para.birth =
                !para.birth || para.birth === ''
                  ? ''
                  : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd')
              editUser(para).then(res => {
                this.$message({
                  message: '提交成功',
                  type: 'success'
                })
                this.$refs['editForm'].resetFields()
                this.dialogFormVisible = false
                this.getUsers()
              })
            })
            .catch(e => {
              // 打印一下错误
              console.log(e)
            })
        }
      })
    },
    // 新增
    createData: function() {
      this.$refs.editForm.validate(valid => {
        if (valid) {
          this.$confirm('确认提交吗？', '提示', {})
            .then(() => {
              this.editForm.id = (parseInt(Math.random() * 100)).toString() // mock a id
              const para = Object.assign({}, this.editForm)
              console.log(para)

              para.birth =
                !para.birth || para.birth === ''
                  ? ''
                  : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd')
              addUser(para).then(res => {
                this.$message({
                  message: '提交成功',
                  type: 'success'
                })
                this.$refs['editForm'].resetFields()
                this.dialogFormVisible = false
                this.getUsers()
              })
            })
            .catch(e => {
              // 打印一下错误
              console.log(e)
            })
        }
      })
    },
    // 全选单选多选
    selsChange(sels) {
      this.sels = sels
    },
    // 批量删除
    batchRemove() {
      var ids = this.sels.map(item => item.id).toString()
      this.$confirm('确认删除选中记录吗？', '提示', {
        type: 'warning'
      })
        .then(() => {
          const para = { ids: ids }
          batchRemoveUser(para).then(res => {
            this.$message({
              message: '删除成功',
              type: 'success'
            })
            this.getUsers()
          })
        })
        .catch(() => {})
    }
  },
  mounted() {
    this.getUsers()
  }
}
</script>

<style scoped>

</style>