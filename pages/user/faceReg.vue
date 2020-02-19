<template>
	<view class="content">
		<view class="status_bar" style="background-color: #f1f1f1;">
		    <!-- 这里是状态栏 -->
		</view>
		<uni-nav-bar left-icon="back" @clickLeft="back()" left-text="返回" background-color="#f8f8f8" title="人脸识别"></uni-nav-bar>
		<view class="padding">
			<view class="list">
				<view class="tishi">点击图片人脸识别</view>
				<view class="">
					<!-- <image class="img-css" src="../../static/imgs/user/gaoxiang.jpg"></image> -->
					<image class="img-css" :src="imageShow" :data-src="imageShow" @tap="previewImage"></image>
				</view>
				<view class="margin-tb-sm text-center">
					
					<button class="cu-btn bg-blue lg shadow" @tap="chooseImage">拍摄</button>
					
				</view>
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
	import permision from "@/common/permission.js"
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	var sourceType = [
		['camera'],
		['album'],
		['camera', 'album']
	]
	var sizeType = [
		['compressed'],
		['original'],
		['compressed', 'original']
	]
	export default {
		components: {uniNavBar},
		data() {
			return {
				title: 'choose/previewImage',
				imageShow:'../../static/imgs/user/gaoxiang.jpg',
				imageList: [],
				sourceTypeIndex: 2,
				sourceType: ['拍照', '相册', '拍照或相册'],
				sizeTypeIndex: 2,
				sizeType: ['压缩', '原图', '压缩或原图'],
				countIndex: 8,
				count: [1, 2, 3, 4, 5, 6, 7, 8, 9]
			}
		},
		onUnload() {
			this.imageList = [],
				this.sourceTypeIndex = 2,
				this.sourceType = ['拍照', '相册', '拍照或相册'],
				this.sizeTypeIndex = 2,
				this.sizeType = ['压缩', '原图', '压缩或原图'],
				this.countIndex = 8;
		},
		methods: {
			back(){
				uni.navigateBack({
					delta:1,
				})
			},
			chooseImage: async function() {
				// #ifdef APP-PLUS
				// TODO 选择相机或相册时 需要弹出actionsheet，目前无法获得是相机还是相册，在失败回调中处理
				if (this.sourceTypeIndex !== 2) {
					let status = await this.checkPermission();
					if (status !== 1) {
						return;
					}
				}
				// #endif

				if (this.imageList.length === 9) {
					let isContinue = await this.isFullImg();
					console.log("是否继续?", isContinue);
					if (!isContinue) {
						return;
					}
				}
				uni.chooseImage({
					sourceType: sourceType[this.sourceTypeIndex],
					sizeType: sizeType[this.sizeTypeIndex],
					count: this.imageList.length + this.count[this.countIndex] > 9 ? 9 - this.imageList.length : this.count[this.countIndex],
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
						this.imageShow=res.tempFilePaths[0];
					},
					fail: (err) => {
						// #ifdef APP-PLUS
						if (err['code'] && err.code !== 0 && this.sourceTypeIndex === 2) {
							this.checkPermission(err.code);
						}
						// #endif
						// #ifdef MP
						uni.getSetting({
							success: (res) => {
								let authStatus = false;
								switch (this.sourceTypeIndex) {
									case 0:
										authStatus = res.authSetting['scope.camera'];
										break;
									case 1:
										authStatus = res.authSetting['scope.album'];
										break;
									case 2:
										authStatus = res.authSetting['scope.album'] && res.authSetting['scope.camera'];
										break;
									default:
										break;
								}
								if (!authStatus) {
									uni.showModal({
										title: '授权失败',
										content: 'Hello uni-app需要从您的相机或相册获取图片，请在设置界面打开相关权限',
										success: (res) => {
											if (res.confirm) {
												uni.openSetting()
											}
										}
									})
								}
							}
						})
						// #endif
					}
				})
			},
			
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: [this.imageShow]
				})
			},
			async checkPermission(code) {
				let type = code ? code - 1 : this.sourceTypeIndex;
				let status = permision.isIOS ? await permision.requestIOS(sourceType[type][0]) :
					await permision.requestAndroid(type === 0 ? 'android.permission.CAMERA' :
						'android.permission.READ_EXTERNAL_STORAGE');

				if (status === null || status === 1) {
					status = 1;
				} else {
					uni.showModal({
						content: "没有开启权限",
						confirmText: "设置",
						success: function(res) {
							if (res.confirm) {
								permision.gotoAppSetting();
							}
						}
					})
				}

				return status;
			}
		}
	}
</script>

<style>
	@import "../../common/common.css";
	.content {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	.atribute-text{
		font-size: 34upx;
	}
	.tishi {
		color: #999999;
		font-size: 28upx;
		line-height: 50upx;
		margin-bottom: 50upx;
		text-align: center;
	}
	.img-css{
		padding: 20upx;
		width: 600upx;
		height: 600upx;
		border-radius: 30upx;
	}
	.list {
		display: flex;
		flex-direction: column;
		padding-top: 50upx;
		padding-left: 70upx;
		padding-right: 70upx;
	}

	.list-call {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		align-items: center;
		height: 100upx;
		color: #333333;
		border-bottom: 1upx solid rgba(230, 230, 230, 1);
	}

	.list-call .img {
		width: 40upx;
		height: 40upx;
	}

	.list-call .biaoti {
		flex: 1;
		text-align: left;
		font-size: 32upx;
		line-height: 100upx;
		margin-left: 16upx;
	}

	.dlbutton {
		color: #FFFFFF;
		font-size: 34upx;
		width: 470upx;
		height: 100upx;
		background: linear-gradient(-90deg, rgba(63, 205, 235, 1), rgba(188, 226, 158, 1));
		box-shadow: 0upx 0upx 13upx 0upx rgba(164, 217, 228, 0.2);
		border-radius: 50upx;
		line-height: 100upx;
		text-align: center;
		margin-left: auto;
		margin-right: auto;
		margin-top: 100upx;
	}

	.dlbutton-hover {
		background: linear-gradient(-90deg, rgba(63, 205, 235, 0.9), rgba(188, 226, 158, 0.9));
	}

	.yzm {
		color: #FF7D13;
		font-size: 24upx;
		line-height: 64upx;
		padding-left: 10upx;
		padding-right: 10upx;
		width: auto;
		height: 64upx;
		border: 1upx solid rgba(255, 131, 30, 1);
		border-radius: 50upx;
	}

	.yzms {
		color: #999999 !important;
		border: 1upx solid #999999;
	}
</style>
