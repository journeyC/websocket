<template>
	<div class="warp">
		<div class="top">
			<div class="logo">
				<img src="../../../src/assets/logo.png" alt="">
			</div>
		</div>
		<div class="cont">
			<Input @inputs="inputs('user')" :plc="'手机号、账号或邮箱'" v-model="user"/>
			<Input @inputs="inputs('pwd')"  :plc="'密码'" v-model="pwd"/>
			<Input @inputs="inputs('pwds')"  :plc="'确认密码'" v-model="pwds"/>
		</div>
		<button class="btn" :class="[onlogin?'on':'']" @keyup.enter="name_reg"  @click="name_reg">注册</button>
		<div class="bot">
			<span>有账户了？</span>
			<router-link to='/login'>登录。</router-link>
		</div>
	</div>
</template>

<script>
import Input from '@/components/input';
import {Toast} from 'mint-ui'
import {name_reg} from '@/api/api.js'
import {validname,validpwd} from '@/api/valid.js'
	export default{
		data(){
			return{
				user:'',
				pwd:'',
				pwds:'',
				onlogin:false,
			}
		},
		methods:{
			inputs(name){
				this[name]='';
			},
			name_reg(){
				var _self=this;
				if(!validname(_self.user)){
					Toast('用户名汉字 英文字母 数字 下划线组成，1-20位！');
					return false;
				}
				if(!validpwd(_self.pwd)){
					Toast('密码3-16位!');
					return false;
				}
				if(_self.pwd!=_self.pwds){
					Toast('密码不一致！')
					return false;
				}
				 name_reg({
		          username:_self.user,
		          userpass:_self.pwd
		        }).then( res => {
		        	if(res.data.status==1){
		        		localStorage.setItem("userinfo",JSON.stringify(res.data.data));
		        		this.$store.dispatch('changelogin');
		        		this.$router.push({path:'/'});
		        		Toast(res.data.msg)
		        	}else{
		        		Toast(res.data.msg)
		        		return false;
		        	}
		        })
			}
		},
	  components: {
	    Input,
	  },
	    watch: {
	      user(val) {
	       if(this.user!=''&&this.pwd!=''&&this.pwds!=''){
	       	this.onlogin=true;
	       }else{
	       	this.onlogin=false;
	       }
	      },
	      pwd(val) {
	        if(this.user!=''&&this.pwd!=''&&this.pwds!=''){
	        	this.onlogin=true;
	        }else{
	        	this.onlogin=false;
	        }
	      },
	      pwds(val) {
	        if(this.user!=''&&this.pwd!=''&&this.pwds!=''){
	        	this.onlogin=true;
	        }else{
	        	this.onlogin=false;
	        }
	      }
	    }
	}
</script>

<style scoped>
	.warp{padding:0 0.4rem;}
	.warp .top{text-align: center;padding:40px 0;}
	.warp .top .logo{width:2.5rem;height:2.5rem;border-radius:50%;overflow: hidden;display: inline-block;}
	.warp .top .logo img{width:100%;height: 100%;}
	.warp .cont .forget{color:#3897f0;font-weight:bold;font-size:0.3rem;}
	.warp .btn{margin-top:0.5rem;}
	.warp .or{border-top:1px solid #e4e4e4;margin:0.8rem 0;text-align: center;}
	.warp .or span{background:#fff;padding:0.1rem 0.2rem;color:#999;position: relative;
    top: -0.3rem;}
    .warp .bot{position: fixed;bottom:0;left:0;height:1.2rem;line-height:1.2rem;border-top:1px solid #e4e4e4;text-align: center;width:100%;color:#b0b0b0;font-size:0.3rem;}
    .warp .bot span{display: inline-block;}
    .warp .bot a{display: inline-block;text-decoration:none;font-size:0.3rem;color:#3897f0;font-weight: bold;}
    .logins{text-align: center;}
    .logins i{font-size:1.1rem;color:#999; font-weight:200; opacity:1;}
    .logins i.iconweixin1{font-size:1.14rem;margin-right:0.2rem;}
</style>