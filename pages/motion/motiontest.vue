<template>
	<view>
		<view class="status_bar">
		    <!-- 这里是状态栏 -->
		</view>
		<view class="">
			<view class="page-body">
				<view class="page-body-wrapper" bindtap="hindCamera">
			
					<view>
						<van-icon name="expand-o" size="50upx" />
					</view>
				</view>
			
				<view>
					<text>开启摄像头</text>
				</view>
			</view>
			<view class="camera-css" bindtap="hindCamera">
			
				<camera device-position="front" flash="off" binderror="error" :class="cameraShow" style="width: 180rpx; height: 250rpx;">
				</camera>
			</view>
			<view class="pageView">
				<view class="video-css">
					<!-- <video id="TestVideo" objectFit="cover" controls="{{false}}" show-fullscreen-btn="{{false}}" enable-progress-gesture="{{false}}"
					 enable-play-gesture="{{true}}" play-btn-position="center" bindended="playEnd" custom-cache="{{false}}" autoplay="{{true}}"
					 :src="videUrl">
					</video> -->
					<video id="TestVideo" objectFit="cover" 
					 play-btn-position="center" bindended="playEnd"
					 :src="videUrl">
					</video>
				</view>
				<view class="btn-css">
					<button class='test-btn' bindtap="testStart">播放</button>
				</view>
				<view class="btn-css speak-btn-css">
					<view class="foot-group" catchlongpress="catchTapEvent">
						<bottom-button :button-disabled="bottomButtonDisabled" bindrecordstart="streamRecord" bindrecordend="streamRecordEnd"></bottom-button>
					</view>
				</view>
			</view>
			
		</view>
			
	</view>

</template>

<script>
</script>

<style>
	@import "../../common/common.css";
</style>
