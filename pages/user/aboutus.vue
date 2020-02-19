<template>
	<view class="content">
		<view class="status_bar" style="background-color: #f1f1f1;">
		    <!-- 这里是状态栏 -->
		</view>
		<uni-nav-bar left-icon="back" @clickLeft="back()" left-text="返回" background-color="#f8f8f8" title="关于我们"></uni-nav-bar>
		<view class="padding">
			<view class="list">
				<view class="tishi">不忘初心，牢记使命</view>
				<view class="list-call">
					<text class="atribute-text">华中科技大学嵌入与普适计算实验室</text>
					<text class="atribute-text">迈思智能科技(武汉)有限公司</text>
				</view>
				<view class="">
					<image class="img-css" src="../../static/imgs/user/gaoxiang.jpg"></image>
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
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		
		components: {uniNavBar},
		methods: {
			back(){
				uni.navigateBack({
					delta:1,
				})
			},
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
