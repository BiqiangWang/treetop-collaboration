<template>
  <div class="login">
    <el-card class="login__card">
      <div slot="header">Login</div>
      <el-form>
        <el-form-item label="User name">
          <el-input v-model="userName"></el-input>
        </el-form-item>
        <el-form-item label="Password">
          <el-input type="password" v-model="password"></el-input>
		  <el-button type="text" class="register" @click="registerVisible = true">注册</el-button>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" v-on:click="login">Login</el-button>
        </el-form-item>
      </el-form>
    </el-card>
	<el-dialog title="TreeTop Register" :visible.sync="registerVisible" width="50%" :modal-append-to-body="false">
		<el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
			<el-form-item label="User name">
			  <el-input v-model="registerUserName"></el-input>
			</el-form-item>
			<el-form-item label="密码" prop="pass">
			  <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item label="确认密码" prop="checkPass">
			  <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item>
		      <el-button type="primary" @click="submitForm('ruleForm')">注册</el-button>
			</el-form-item>
		</el-form>
	</el-dialog>
  </div>
</template>

<script>
export default{
  name: 'Login',
  data () {
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
    return {
	  registerVisible: false,
      userName: 'test-user1',
	  registerUserName: 'test-user',
      password: '',
      ruleForm: {
          pass: '',
          checkPass: ''
        },
        rules: {
          pass: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ]
        }
    }
  },
  methods: {
    submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
			  this.register();
            // alert('欢迎加入树梢协作!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
    }, 
    login () {
      // 登录，获取用户信息，后获取所有项目
      this.axios.get('http://39.97.175.119:8801/user/login', {
        params: {
          name: this.userName,
          password: this.password
        }
      })
      .then((response) => {
        if(response.data.message == '成功'){
          this._GLOBAL.userID = response.data.data.user.ID
          this._GLOBAL.userObj = response.data.data.user
          this.$router.push('/' + this._GLOBAL.userObj.name)
          // 获取所有项目
          // this.axios.get('http://39.97.175.119:8801/user/login', {
          //   params: {
          //     userID: this._GLOBAL.userObj.ID
          //   }
          // })
          // .then((response) => {
          //   console.log(response);
          //   if(response.data.message == '成功'){
          //     this._GLOBAL.ProjectList =
          //   }
          // }
        }
        else{
          this.$alert('The login failed, please re-enter your username and password', 'Login Failed', { confirmButtonText: 'OK' })
        }
      })
      .catch(function (error) {
        console.log(error);
      });
    },
	register(){
		// 'headers': {'Access-Control-Allow-Origin': '*','access-control-allow-credentials': 'true'}
		// this.axios.setRequestHeader()
		this.axios.post('/api/user/register',{
			name: this.registerUserName,
			password: this.ruleForm.pass,
		},{
            emulateJSON:true
        })
		.then((response) =>{
			if(response.data.message == '成功'){
				this.registerVisible=false;
				this.$alert('欢迎加入树梢协作!');
			}else{
				console.log(response)
				this.$alert(response.data.message)
			}
		})

	},
  }
}
</script>

<style>
  .login{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
  }
  .login__card{
    width: 300px;
  }
  .register{
	  margin-left: 210px;
  }
</style>
