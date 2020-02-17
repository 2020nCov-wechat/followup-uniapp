<template>
	<view class="bg-gradual-blue">
		<scroll-view scroll-y class="DrawerPage" :class="modalName=='viewModal'?'show':''">
			<view class='user'>
				<view class='header bg-color acea-row row-between-wrapper '>
					<view class='header-have-arrow'>
						<view class="setting-view">
							<image class="setting-img" src="../../static/imgs/setting.png" @tap="showModal" data-target="viewModal"></image>
						</view>
						<view class="header-out-style">
							<view class='picTxt acea-row row-between-wrapper header-display '>
								<view class='pictrue'>
									<image bindtap="bindViewTap" class="userinfo-avatar" src="../../static/imgs/avatar.jpeg" mode="cover"></image>
								</view>
								<view class=''>
									<view class='acea-row row-middle name-css'>
										<view class='name'>庄兄</view>
									</view>
								</view>
							</view>
						</view>
					</view>
				</view>

				<view class='wrapper'>
					<view class='nav acea-row row-middle'>
						<view class='item' data-url='/pages/user_money/index'>
							<view bindtap="updateChartClick" class="btn-color">刷新综合评价</view>
						</view>

						<view class='item' data-url='/pages/user_coupon/index'>
							<view bindtap="getOpenIdTabFromAPP" class="btn-color">刷新登录状态</view>
							<!-- <view class='num'>{{userInfo.couponCount || 0}}</view> -->
						</view>
					</view>

					<view class='myService'>
						<view class='title acea-row row-middle'>综合评价</view>
						<view class='serviceList acea-row row-middle'>

							<view class="result-img-view">
								<view class="img-view">
									<image class='result-img' :src="imgPath"></image>
								</view>
							</view>
							<view class="suggess-css">
								<text class="text-black">评估建议：{{suggestion}}</text>
							</view>

						</view>
					</view>

				</view>

			</view>


		</scroll-view>
		<view class="DrawerClose" :class="modalName=='viewModal'?'show':''" @tap="hideModal">
			<text class="cuIcon-pullright"></text>
		</view>
		<scroll-view scroll-y class="DrawerWindow" :class="modalName=='viewModal'?'show':''">
			<view class="cu-list menu card-menu margin-top-xl margin-bottom-xl shadow-lg">

				<view class="grid margin-bottom text-centerb col-2">
					<view class="padding">
						<view class="cu-avatar xl round" style="background-image:url(../../static/imgs/avatar.jpeg);"></view>
					</view>
				</view>

				<view class="cu-item arrow">
					<view class="content">
						<text class="cuIcon-people text-grey"></text>
						<text class="text-grey">个人中心</text>
					</view>
				</view>
				<view class="cu-item arrow">
					<view class="content">
						<text class="cuIcon-edit text-grey"></text>
						<text class="text-grey">意见反馈</text>
					</view>
				</view>
				<view class="cu-item arrow">
					<view class="content">
						<text class="cuIcon-pullup text-grey"></text>
						<text class="text-grey">版本更新</text>
					</view>
				</view>
				<view class="cu-item arrow">
					<view class="content">
						<text class="cuIcon-github text-grey"></text>
						<text class="text-grey">关于我们</text>
					</view>
				</view>


			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				modalName: null,
				imgPath: '../../static/imgs/result/noResult.png',
				suggestion: '早睡早起，吃好喝好。'
			};
		},
		methods: {
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			}
		},
	}
</script>

<style>
	@import "../../common/common.css";

	page {
		background-image: var(--gradualBlue);
		width: 100vw;
		overflow: hidden;
	}

	.DrawerPage {
		position: fixed;
		width: 100vw;
		height: 100vh;
		left: 0vw;
		background-color: #f1f1f1;
		transition: all 0.4s;
	}

	.DrawerPage.show {
		transform: scale(0.9, 0.9);
		left: 85vw;
		box-shadow: 0 0 60upx rgba(0, 0, 0, 0.2);
		transform-origin: 0;
	}

	.DrawerWindow {
		position: absolute;
		width: 85vw;
		height: 100vh;
		left: 0;
		top: 0;
		transform: scale(0.9, 0.9) translateX(-100%);
		opacity: 0;
		pointer-events: none;
		transition: all 0.4s;
		padding: 100upx 0;
	}

	.DrawerWindow.show {
		transform: scale(1, 1) translateX(0%);
		opacity: 1;
		pointer-events: all;
	}

	.DrawerClose {
		position: absolute;
		width: 40vw;
		height: 100vh;
		right: 0;
		top: 0;
		color: transparent;
		padding-bottom: 120upx;
		display: flex;
		align-items: flex-end;
		justify-content: center;
		background-image: linear-gradient(90deg, rgba(0, 0, 0, 0.01), rgba(0, 0, 0, 0.6));
		letter-spacing: 5px;
		font-size: 70upx;
		opacity: 0;
		pointer-events: none;
		transition: all 0.4s;
	}

	.DrawerClose.show {
		opacity: 1;
		pointer-events: all;
		width: 15vw;
		color: #fff;
	}

	.DrawerPage .cu-bar.tabbar .action button.cuIcon {
		width: 64upx;
		height: 64upx;
		line-height: 64upx;
		margin: 0;
		display: inline-block;
	}

	.DrawerPage .cu-bar.tabbar .action .cu-avatar {
		margin: 0;
	}

	.DrawerPage .nav {
		flex: 1;
	}

	.DrawerPage .nav .cu-item.cur {
		border-bottom: 0;
		position: relative;
	}

	.DrawerPage .nav .cu-item.cur::after {
		content: "";
		width: 10upx;
		height: 10upx;
		background-color: currentColor;
		position: absolute;
		bottom: 10upx;
		border-radius: 10upx;
		left: 0;
		right: 0;
		margin: auto;
	}

	.DrawerPage .cu-bar.tabbar .action {
		flex: initial;
	}
</style>
