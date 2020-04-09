<template>
  <div>
		<div class="bj">
			<img src="../public/img/bj.png" alt="" width="100%">
		</div>
		<div class="login center">
			<div style="position: relative;width: 600px;height: 250px;">
				<div class="center" style="width: 80%;">
					<div class="input-group ipt">
						<input type="text"  class="form-control deal" placeholder="请输入用户名" aria-describedby="basic-addon2" v-model="user" @change='check()'>
					</div>
					<div class="input-group ipt">
						<input type="password" class="form-control" placeholder="请输入密码" aria-describedby="basic-addon2" v-model="psd" >
					</div>
					<div style="width: 80%;display: flex;justify-content: space-around;text-align: center;margin: 0 auto;">
						<button class='obtain'  @click='login()'>
							登录
						</button>
						<button class='obtain'   @click='confirm'>
							注册
						</button>
					</div>
				</div>
			</div>
		</div>
  </div>
</template>
<script>
import { userLogin } from '../vuex/actions'
import vuex from 'vuex'
import CHAT from '../client'
import settings from '../settings.js'
export default {
  data(){
    return {
      user: '',
      psd: ''
    }
  },
  methods: {
    login() {
      let user = {
        username: this.user,
        password: this.psd
      }
			if(this.user!=''&&this.psd!=''){
				this.$http.post(settings.server+'/login', user).then((res) => {
				  var result = res.body
				  var username = result.data.user.username
				  if(result.status === 'OK') {
				    this.userLogin(username)
				    CHAT.init(username)
				    this.$router.go('/user/userlist/noconnect')
				  } else {
				    alert(result.msg)
				  }
				})
			}else{
				alert('请输入完整信息')
			}
    },
    confirm() {
      this.$router.go('/confirm')
    }
  },
  ready() {
    document.onkeydown = (e) => {
      if(e && e.keyCode == 13) {
        this.login()
      }
    }
  },
  vuex: {
    actions: {
      userLogin: userLogin
    }
  }
}
</script>

<style scoped> 
.bj{
	position: absolute;
	width: 100%;
	height: 100%;
	overflow: hidden;
}
.center{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%,-50%);
}
.login{
	width: 600px;
	height: 300px;
	background: rgba(255,255,255,0.6);
	border-radius:30px;
}
.login>div{
	display: flex;
	width: 60%;
	margin: 20px auto;
}
button{
	border: none ;
	color: white;
	outline: none;
	height: 50px;
	border-radius: 20px !important;
}
input{
	border: none;
	background:rgba(255,255,255,0.7);
	outline: none;
	width: 80%;
}
.obtain{
	line-height: 30px;
	text-align: center;
	width: 25%;
	background: rgba(21,133,255,0.6);
	border-radius: 0 5px 5px 0;
	display: inline-block;
	color: white;
	border: none;
}
.ipt{
	margin: 20px 0;
	text-align: center;
}
.form-control{
	border-radius: 5px;
	height: 50px;
	margin: 0 auto;
	padding-left: 20px;
}
</style>
