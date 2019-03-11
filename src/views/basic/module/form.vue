<template>
    <div class="app-container">
        <!--
            <div class="crumbs">
                <el-breadcrumb separator="/">
                    <el-breadcrumb-item><i class="el-icon-date"></i> 表单</el-breadcrumb-item>
                    <el-breadcrumb-item>基本表单</el-breadcrumb-item>
                </el-breadcrumb>
            </div>
        -->
        <div class="container">
            <div class="form-box">
                <el-form ref="form" :model="form" label-width="80px">
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="模块编码">
                                <el-input v-model="form.name"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="模块名称">
                                <el-input v-model="form.name"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>

                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="模板类型">
                            <el-select v-model="form.region" placeholder="请选择">
                                    <el-option key="bbk" label="基础模板" value="bbk"></el-option>
                                    <el-option key="xtc" label="增值模板" value="xtc"></el-option>
                                </el-select>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="单价">
                                <el-input v-model="form.name" type="number"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>

                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="状态">
                            <el-select v-model="form.region" placeholder="请选择">
                                    <el-option key="bbk" label="保存" value="bbk"></el-option>
                                    <el-option key="xtc" label="启用" value="xtc"></el-option>
                                    <el-option key="imoo" label="禁用" value="imoo"></el-option>
                                </el-select>
                            </el-form-item>
                        </el-col>
                    </el-row>

                    <el-row>
                        <el-col :span="24">
                            <el-form-item label="备注">
                                <el-input type="textarea" rows="3" v-model="form.desc"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>

                    <el-row>
                        <el-col :span="24">
                            <el-form-item>
                                <el-button type="primary" @click="onSubmit">提交</el-button>
                                <el-button>取消</el-button>
                            </el-form-item>
                        </el-col>
                    </el-row>

                </el-form>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
      name: 'baseform',
      data: function() {
        return {
          form: {
            id:'' ,
            number: '',
            name: '',
            type: '',
            price: undefined,
            status:'',
            remark: '小天才',
            createUser: '',
            createTime: '',
            lastUpdateUser:'',
            lastUpdateTime:''
          }
        }
      },
      methods: {
        onSubmit() {
          this.$message.success('提交成功！')
        }
      }
    }
</script>

<style scope>
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>
