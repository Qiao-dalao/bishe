<template>
	<div>
		<div>
			<div class="bj">
				<img src="../public/img/bj.png" alt="" width="100%">
			</div>
			<div class="login center">
				<div style="position: relative;width: 600px;height: 250px;">
					<div class="center" style="width: 80%;">
						<div style="position: relative;">
							<input type="file" @change='fileChange' class="filePut">
							<div class="toux">
								<img src="../public/img/userImg.png" alt="" class="touxDetails">
							</div>
						</div>
						<div class="input-group ipt">
							<input type="text"  class="form-control deal" placeholder="请输入用户名" aria-describedby="basic-addon2" v-model="user" @change='check()'>
						</div>
						<div class="input-group ipt">
							<input type="password" class="form-control" placeholder="请输入密码" aria-describedby="basic-addon2" v-model="psd" >
						</div>
						<div class="input-group ipt">
							<input type="password" class="form-control" placeholder="请再次输入密码" aria-describedby="basic-addon2" v-model="psd_r" >
						</div>
						<div style="width: 80%;display: flex;justify-content: space-around;text-align: center;margin: 0 auto;">
							<button class='obtain'   id='zhuce' @click='login()'>
								注册
							</button>
							<button class='obtain'   @click='goLoginform'>
								登录
							</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
import { userLogin } from '../vuex/actions'
import settings from '../settings.js'
import vuex from 'vuex'
export default {
  data: function(){
    return {
        fileSrc: null,
        user: '',
        psd: '',
        psd_r: '',
        url: '',
        download_url: null,
        submitStatus:'点击提交',
				touxiang:''
    }
  },
  methods: {
			login() {
				if(this.psd !== this.psd_r || !this.psd || !this.psd_r || !this.user) {
					alert('填写信息有误')
				} else {
					var formData = new FormData()
					formData.append('username',this.user)
					formData.append('username',this.user)
					axios({
						method:'post',
						data:formData,
						url:settings.server + '/addUserImg',
					})
					// var options = {
					//     type: 'post',
					//     url: settings.server + '/addUserImg',
					//     dataType: 'json',
					// 		data:{
					// 			username:this.user,
					// 			touxiang:this.touxiang
					// 		},
					//     contentType:"multipart/form-data",
					//     success:(ret) => { 
					//       console.log('提交成功')
					//     },
					//     error:(ret) => {
					//       console.log('提交失败')
					//     }
					// };
					// $('#zhuce').ajaxForm(options).submit()
						
					var user = {
						username: this.user,
						password: this.psd,
						password_r: this.psd_r,
						touxiang:this.touxiang
					}
					this.$http.post(settings.server + '/addUser', user).then((res) => {
						var result = res.body
						if(result.status === 'OK') {
							this.$router.go('/')
						} else {
							alert(result.msg)
						}
					})
				}
			},
			goLoginform(){
				this.$router.go('/')
			},
			fileChange(event){
				var touxDetails = document.querySelector('.touxDetails')
				this.touxiang = event.target.files[0]
				var file = new FileReader()
				file.readAsDataURL(event.target.files[0])
				file.onload = function(e){
					touxDetails.src = e.target.result
					
				}
			}
  },
  ready: function() {
    document.onkeydown = (e) => {
      if(e && e.keyCode == 13) {
        this.login()
      }
    }
    this.$http.get('http://123.206.95.98:9002/node/').then((data) => {
      let sign = data.body.sign
      let url = data.body.url + '?sign=' + encodeURIComponent(sign);
      this.url = url
       
        // pass options to ajaxForm 
        
    })
  },
  vuex: {
    actions: {
    }
  }
}
</script>
<style scoped> 
.toux{
	width: 80px;
	margin: 0 auto;
	margin-top: 120px;
	height: 80px;
	overflow: hidden;
}
.toux img{
	width: 100%;
}
.filePut{
	width: 50px;
	height: 50px;
	position: absolute;
	top: 0;
	left:50%;
	transform: translate(-50%,0);
	opacity: 0;
}
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
	height: 400px;
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