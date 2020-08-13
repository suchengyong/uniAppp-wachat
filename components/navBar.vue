<template>
	<view>
		<view :class="getClass">
			<!-- 状态栏 -->
			<view :style="'height:'+statusBarHeight+'px'"></view>
			<!-- 导航 -->
			<view class="w-100 flex align-center justify-between" style="height: 90rpx;">
				<!-- 左边 -->
				<view class="flex align-center">
					<!-- 返回按钮 -->
					<icon-button v-if="showBack"><navigator animation-type="pop-out" animation-duration="300" open-type="navigateBack" ><text class="iconfont font-md">&#xe60d;</text></navigator></icon-button>
					<!-- 标题 -->
					<text v-if="title" class="font-md" :class="showBack?'':'ml-3'">{{getTitle}}</text>
				</view>
				<!-- 右边 -->
				<view class="flex align-center">
					<slot name="right">
						<icon-button @click="search"><text class="iconfont font-md">&#xe6e3;</text></icon-button>
						<icon-button @click="openExtend"><text class="iconfont font-md">&#xe682;</text></icon-button>
					</slot>
				</view>
			</view>
		</view>
		<!-- 占位 -->
		<view v-if="fixed" :style="fixedStyle"></view>
		
		<!-- 扩展菜单 -->
		<popup ref="extendNav" :bodyWidth="320" :bodyHeight="525"
		bodyBgColor="bg-dark" transformOrigin="right top">
			<view class="flex flex-column bg-dark" 
			style="width: 320rpx;height: 525rpx;">
				<view class="flex-1 flex align-center" 
				hover-class="bg-hover-dark"
				v-for="(item,index) in menus"
				:key="index"
				@click="clickEvent(item.event)">
					<text class="iconfont pl-3 pr-2 font-md text-white">{{item.icon}}</text>
					<text class="font-md text-white">{{item.name}}</text>
				</view>
			</view>
		</popup>
	</view>
</template>

<script>
	import iconButton from './iconButton.vue';
	import popup from "./popup.vue";
	export default {
		props: {
			title: {
				type: [String,Boolean],
				default:false 
			},
			fixed:{
				type:Boolean,
				default:true
			},
			noreadnum:{
				type:Number,
				default:0
			},
			bgcolor:{
				type:String,
				default:'bg-white'
			},
			showBack:{
				type:Boolean,
				default:false
			}
		},
		components:{
			iconButton,
			popup
		},
		data() {
			return {
				statusBarHeight:0,
				navBarHeight:0,
				menus:[
					{
						name:"发起群聊",
						event:"groupChat",
						icon:"\ue633"
					},
					{
						name:"添加好友",
						event:"addFriend",
						icon:"\ue65d"
					},
					{
						name:"扫一扫",
						event:"scan",
						icon:"\ue614"
					},
					{
						name:"收付款",
						event:"payment",
						icon:"\ue66c"
					},
					{
						name:"帮助与反馈",
						event:"help",
						icon:"\ue66c"
					}
				],
			}
		},
		mounted() {
			// #ifdef APP-PLUS-NVUE
			this.statusBarHeight = plus.navigator.getStatusbarHeight()
			// #endif
			this.navBarHeight = this.statusBarHeight + uni.upx2px(90)
		},
		computed: {
			fixedStyle() {
				return `height:${this.navBarHeight}px`
			},
			getTitle(){
				let noreadnum = this.noreadnum > 0 ? '('+this.noreadnum+')' : ''
				return this.title + noreadnum
			},
			getClass(){
				let fixed = this.fixed?'fixed-top':''
				return `${this.bgcolor} ${fixed}`
			}
		},
		methods: {
			openExtend() {
				this.$refs.extendNav.show(uni.upx2px(415),uni.upx2px(150))
			},
			clickEvent(event) {
				console.log(event)
				switch (event) {
					case "groupChat":
						break;
					case "addFriend": 
						break;
					case "scan":
						break;
					case "payment": 
						break;
					case "help":
						break;
				}
				this.$refs.extendNav.hide()
			},
			// 搜索
			search() {
				uni.navigateTo({
					url: '/pages/search/search',
				});
			}
		},
	}
</script>

<style>
</style>
