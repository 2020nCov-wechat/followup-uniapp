<template>
	<view class="content">
		<view class="status_bar" style="background-color: #f1f1f1;">
		    <!-- 这里是状态栏 -->
		</view>
		<uni-nav-bar left-icon="back" @clickLeft="back()" left-text="返回" background-color="#f8f8f8" title="忘记密码"></uni-nav-bar>
		<view class="padding">
		<view class="header">
			<image src="../../static/imgs/shilu-login/logo.png"></image>
		</view>
		
		<view class="list">
			<view class="list-call">
				<image class="img" src="/static/imgs/shilu-login/1.png"></image>
				<input class="biaoti" v-model="phoneno" type="number" maxlength="11" placeholder="手机号" />
			</view>
			<view class="list-call">
				<image class="img" src="/static/imgs/shilu-login/3.png"></image>
				<input class="biaoti" v-model="code" type="number" maxlength="4" placeholder="验证码" />
				<view class="yzm" :class="{ yzms: second>0 }" @tap="getcode">{{yanzhengma}}</view>
			</view>
			<view class="list-call">
				<image class="img" src="/static/imgs/shilu-login/2.png"></image>
				<input class="biaoti" v-model="password" type="text" maxlength="32" placeholder="登录密码" :password="!showPassword" />
				<image class="img" :src="showPassword?'/static/imgs/shilu-login/op.png':'/static/shilu-login/cl.png'" @tap="display"></image>
			</view>
			
			<view class="list-call">
				<image class="img" src="/static/imgs/shilu-login/4.png"></image>
				<input class="biaoti" v-model="invitation" type="text" maxlength="12" placeholder="学号" />
			</view>
			<view class="list-call">
				<image class="img" src="/static/imgs/shilu-login/5.png"></image>
				<picker class="biaoti" @change="bindPickerChange" :value="index" :range="array" range-key="name">
					<view class="uni-input" :class="inputFlag?'text-grey':''">{{inputFlag?'学校':array[index].name}}</view>
				</picker>
			</view>
			
		</view>
		
		<view class="dlbutton" hover-class="dlbutton-hover" @tap="bindLogin">
			<text>注册</text>
		</view>
		
		<view class="xieyi">
			<image @tap="xieyitong" :src="xieyi==true?'/static/imgs/shilu-login/ty1.png':'/static/imgs/shilu-login/ty0.png'"></image>
			<text @tap="xieyitong"> 同意</text>
			<navigator url="blog?id=1" open-type="navigate">《软件用户协议》</navigator>
		</view>
		</view>
		<view class="bottom-view ">
			<view class='wrapper'>
				<view class='copy-view'>
					<view class="copy-item">Software Support @ Huazhong</view>
					<view class="copy-item">University ofScience and Technology, </view>
					<view class="copy-item">Embedded and Pervasive Computing (EPIC) Lab</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	var tha,js;
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		onLoad(){
			tha = this;
			
		},
		components: {uniNavBar},
		onUnload(){
			clearInterval(js)
			this.second = 0;
		},
		data() {
			return {
				phoneno:'',
				password:'',
				code:'',
				invitation:'',
				xieyi:true,
				showPassword:false,
				second:0,
				array: [{name:'华中科技大学'},{name: '武汉大学'}, {name:'武汉理工大学'}, {name:'华中师范大学'}],
				index: 0,
				inputFlag:true,
			};
		},
		computed:{
			yanzhengma(){
				if(this.second==0){
					return '获取验证码';
				}else{
					if(this.second<10){
						return '重新获取0'+this.second;
					}else{
						return '重新获取'+this.second;
					}
				}
			}
		},
		methods: {
			back(){
				uni.navigateBack({
					delta:1,
				})
			},
			display() {
			    this.showPassword = !this.showPassword
			},
			xieyitong(){
				this.xieyi = !this.xieyi;
			},
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为：' + e.target.value)
				this.index = e.target.value
				this.inputFlag=false
			},
			getcode(){
				if(this.second>0){
					return;
				}
				this.second = 60;
				uni.request({
				    url: 'http://***/getcode.html', //仅为示例，并非真实接口地址。
				    data: {phoneno:this.phoneno,code_type:'reg'},
					method: 'POST',
					dataType:'json',
				    success: (res) => {
						if(res.data.code!=200){
							uni.showToast({title:res.data.msg,icon:'none'});
						}else{
							uni.showToast({title:res.data.msg});
							js = setInterval(function(){
								tha.second--;
								if(tha.second==0){
									clearInterval(js)
								}
							},1000)
						}
				    }
				});
			},
		    bindLogin() {
				if (this.xieyi == false) {
				    uni.showToast({
				        icon: 'none',
				        title: '请先阅读《软件用户协议》'
				    });
				    return;
				}
				if (this.phoneno.length !=11) {
				    uni.showToast({
				        icon: 'none',
				        title: '手机号不正确'
				    });
				    return;
				}
		        if (this.password.length < 6) {
		            uni.showToast({
		                icon: 'none',
		                title: '密码不正确'
		            });
		            return;
		        }
				if (this.code.length != 4) {
				    uni.showToast({
				        icon: 'none',
				        title: '验证码不正确'
				    });
				    return;
				}
				uni.request({
				    url: 'http://***/reg.html',
				    data: {
						phoneno:this.phoneno,
						password:this.password,
						code:this.code,
						invitation:this.invitation
					},
					method: 'POST',
					dataType:'json',
				    success: (res) => {
						if(res.data.code!=200){
							uni.showToast({title:res.data.msg,icon:'none'});
						}else{
							uni.showToast({title:res.data.msg});
							setTimeout(function(){
								uni.navigateBack();
							},1500) 
						}
				    }
				});
				
		    }
		}
	}
</script>

<style>
	@import "../../common/common.css";
	.content {
		display: flex;
		flex-direction: column;
		justify-content:center;
	}
	.header {
		width:161upx;
		height:161upx;
		background:rgba(63,205,235,1);
		box-shadow:0upx 12upx 13upx 0upx rgba(63,205,235,0.47);
		border-radius:50%;
		margin-top: 30upx;
		margin-left: auto;
		margin-right: auto;
	}
	.header image{
		width:161upx;
		height:161upx;
		border-radius:50%;
	}
	
	.list {
		display: flex;
		flex-direction: column;
		padding-top: 50upx;
		padding-left: 70upx;
		padding-right: 70upx;
	}
	.list-call{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height: 100upx;
		color: #333333;
		border-bottom: 1upx solid rgba(230,230,230,1);
	}
	.list-call .img{
		width: 40upx;
		height: 40upx;
	}
	.list-call .biaoti{
		flex: 1;
		text-align: left;
		font-size: 32upx;
		line-height: 100upx;
		margin-left: 16upx;
	}
	.yzm {
		color: #FF7D13;
		font-size: 24upx;
		line-height: 64upx;
		padding-left: 10upx;
		padding-right: 10upx;
		width:auto;
		height:64upx;
		border:1upx solid #FFA800;
		border-radius: 50upx;
	}
	.yzms {
		color: #999999 !important;
		border:1upx solid #999999;
	}
	.dlbutton {
		color: #FFFFFF;
		font-size: 34upx;
		width:470upx;
		height:100upx;
		background:linear-gradient(-90deg,rgba(63,205,235,1),rgba(188,226,158,1));
		box-shadow:0upx 0upx 13upx 0upx rgba(164,217,228,0.2);
		border-radius:50upx;
		line-height: 100upx;
		text-align: center;
		margin-left: auto;
		margin-right: auto;
		margin-top: 100upx;
	}
	.dlbutton-hover {
		background:linear-gradient(-90deg,rgba(63,205,235,0.9),rgba(188,226,158,0.9));
	}
	.xieyi{
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		font-size: 30upx;
		margin-top: 80upx;
		color: #FFA800;
		text-align: center;
		height: 40upx;
		line-height: 40upx;
	}
	.xieyi image{
		width: 40upx;
		height: 40upx;
	}
</style>
