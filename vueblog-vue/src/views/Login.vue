<template>
  <div class="login" :style="'background-image:url('+ Background +');'">



    <el-container>
      <el-header style="height: 100px;">
        <img class="mlogo" src="@/assets/images/banner-school.png">
        <img class="appname" src="@/assets/images/banner-text.png">
      </el-header>
      <el-main>
        <el-form ref="ruleForm" :model="ruleForm" :rules="rules" label-position="left" label-width="0px" class="login-form">
          <h3>
            学生成绩后台管理系统
          </h3>
          <el-form-item label="用户名" prop="username">
            <el-input v-model="ruleForm.username" type="text" auto-complete="off" placeholder="账号">
              <svg-icon slot="prefix" icon-class="user" class="el-input__icon input-icon" />
            </el-input>
          </el-form-item>
        </el-form>
      </el-main>
    </el-container>

  </div>
</template>

<script>
  import Background from '@/assets/images/bg.png'

  export default {
    name: "Login",
    data() {
      return {
        Background: Background,
        ruleForm: {
          username: 'markerhub',
          password: '111111'
        },
        rules: {
          username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '请选择密码', trigger: 'change' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            const _this = this
            this.$axios.post('/login', this.ruleForm).then(res => {

              console.log(res.data)
              const jwt = res.headers['authorization']
              const userInfo = res.data.data

              // 把数据共享出去
              _this.$store.commit("SET_TOKEN", jwt)
              _this.$store.commit("SET_USERINFO", userInfo)

              // 获取
              console.log(_this.$store.getters.getUser)

              _this.$router.push("/blogs")
            })

          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss">
  .el-header, .el-footer {
    background-color: #ffffff;
    color: #333;
    text-align: center;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
  }
  .login {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    background-size: cover;
  }

  .login-form {
    border-radius: 6px;
    background: #ffffff;
    text-align: center;
    width: 400px;
    margin: auto;
    padding: 25px 25px 5px 25px;
  }


  .mlogo {
    margin-top: 30px;
  }

  .appname {
     margin-top: 35px;
     margin-left: 20px;
   }



</style>