<template>
	<view class="bg-gradual-blue">
		<scroll-view scroll-y class="DrawerPage" :class="modalName=='viewModal'?'show':''">
			<view class='user'>
				<view class='header bg-color acea-row row-between-wrapper'>
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
						<view class='title acea-row row-middle'>结果展示</view>
						<view class='serviceList acea-row row-middle'>

							<view class="qiun-charts">
								<!--#ifdef MP-ALIPAY -->
								<canvas canvas-id="canvasColumn" id="canvasColumn" class="charts" :width="cWidth*pixelRatio" :height="cHeight*pixelRatio"
								 :style="{'width':cWidth+'px','height':cHeight+'px'}" @touchstart="touchColumn"></canvas>
								<!--#endif-->
								<!--#ifndef MP-ALIPAY -->
								<canvas canvas-id="canvasColumn" id="canvasColumn" class="charts" @touchstart="touchColumn"></canvas>
								<!--#endif-->
							</view>

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
				<view class="cu-item arrow" @click="advice()">
					<view class="content">
						<text class="cuIcon-edit text-grey"></text>
						<text class="text-grey">意见反馈</text>
					</view>
				</view>
				<view class="cu-item arrow" @click="checkUpdate()">
					<view class="content">
						<text class="cuIcon-pullup text-grey"></text>
						<text class="text-grey">版本更新</text>
					</view>
				</view>
				<view class="cu-item arrow" @click="aboutUs()">
					<view class="content">
						<text class="cuIcon-github text-grey"></text>
						<text class="text-grey">关于我们</text>
					</view>
				</view>
				<view class="cu-item arrow" @click="toLogin()">
					<view class="content">
						<text class="cuIcon-github text-grey"></text>
						<text class="text-grey">登录-测试</text>
					</view>
				</view>


			</view>
		</scroll-view>
	</view>
</template>

<script>
	import uCharts from '../../components/u-charts/u-charts.js';
	var _self;
	var canvaColumn = null;
	export default {
		data() {
			return {
				modalName: null,
				imgPath: '../../static/imgs/result/noResult.png',
				suggestion: '早睡早起，吃好喝好。',
				cWidth: '',
				cHeight: '',
				pixelRatio: 1,
				textarea: ''
			};
		},
		onLoad() {
			_self = this;
			//#ifdef MP-ALIPAY
			uni.getSystemInfo({
				success: function(res) {
					if (res.pixelRatio > 1) {
						//正常这里给2就行，如果pixelRatio=3性能会降低一点
						//_self.pixelRatio =res.pixelRatio;
						_self.pixelRatio = 2;
					}
				}
			});
			//#endif
			this.cWidth = uni.upx2px(700);
			this.cHeight = uni.upx2px(500);
			this.getServerData();
		},
		methods: {

			toLogin(){
				uni.navigateTo({
					url:'login'
				})
			},
			checkUpdate(){
				uni.showToast({
					title:"已经是最新版本"
				})
			},
			aboutUs(){
				uni.navigateTo({
					url:'aboutus'
				})
			},
			advice(){
				uni.showToast({
					title:"敬请期待！"
				})
			},
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 60
			},
			getServerData() {
				uni.request({
							url: 'http://jsonplaceholder.typicode.com/users',
							data: {},
							success: function(res) {
								console.log(res.data)
								let Column = {
									"categories": [],
									"series": [{
									  "name": "",
									  "data": []
									}]
								};
								//这里我后台返回的是数组，所以用等于，如果您后台返回的是单条数据，需要push进去
								// Column.categories = res.data.data.ColumnB.categories;
								// Column.series = res.data.data.ColumnB.series;
								if (res != null) {
									for (var i = 0; i < 6; i++) {
										
										Column.categories.push(res.data[i].name); //挨个取出类别并填入类别数组
										Column.series[0].data.push(res.data[i].id); //挨个取出销量并填入销量数组
									}
									//Column.series=[35,8,25,37,4,20];[0].name="得分"
									Column.series[0].name="得分";
				                }

									console.log(Column.series);
									//_self.textarea = JSON.stringify(res.data.data.ColumnB);
									_self.showColumn("canvasColumn", Column);
								},
								fail: () => {
									_self.tips = "网络错误，小程序端请检查合法域名";
								},
							});
					},
					showColumn(canvasId, chartData) {
						canvaColumn = new uCharts({
							$this: _self,
							canvasId: canvasId,
							type: 'column',
							padding: [15, 5, 0, 15],
							legend: {
								show: true,
								padding: 5,
								lineHeight: 11,
								margin: 0,
							},
							fontSize: 11,
							background: '#FFFFFF',
							pixelRatio: _self.pixelRatio,
							animation: true,
							categories: chartData.categories,
							series: chartData.series,
							xAxis: {
								disableGrid: true,
						        rotateLabel: true
							},
							yAxis: {
								data: [{
									position: 'left',
									axisLine: false,
									format: (val) => {
										return val.toFixed(0) + '分'
									},
									min:0,
									
								}]
							},
							dataLabel: true,
							width: _self.cWidth * _self.pixelRatio,
							height: _self.cHeight * _self.pixelRatio,
							extra: {
								column: {
									type: 'group',
									width: _self.cWidth * _self.pixelRatio * 0.45 / chartData.categories.length
								}
							}
						});

					},
					touchColumn(e) {
						canvaColumn.showToolTip(e, {
							format: function(item, category) {
								if (typeof item.data === 'object') {
									return category + ' ' + item.name + ':' + item.data.value
								} else {
									return category + ' ' + item.name + ':' + item.data
								}
							}
						});
						// canvaColumn.touchLegend(e, {
						// 	animation: true
						// });
					},
					changeData() {
						if (isJSON(_self.textarea)) {
							let newdata = JSON.parse(_self.textarea);
							canvaColumn.updateData({
								series: newdata.series,
								categories: newdata.categories,
								animation: true
							});
						} else {
							uni.showToast({
								title: '数据格式错误',
								image: '../../../static/images/alert-warning.png'
							})
						}
					}
			},
		}
</script>

<style>
	@import "../../common/common.css";

	/*样式的width和height一定要与定义的cWidth和cHeight相对应*/
	.qiun-charts {
		width: 750upx;
		height: 500upx;
		background-color: #FFFFFF;
	}

	.charts {
		width: 700upx;
		height: 500upx;
		background-color: #FFFFFF;
	}

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
