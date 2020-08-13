<template>
	<scroll-view :show-scrollbar="false" :scroll-with-animation="true" scroll-y="true" :scroll-anchoring="true"	 class="px-3 bg_pray position-fixed left-0 right-0" :style="chartBodyBottom">
		<!-- 聊天信息列表 -->
		<div v-for="(item,index) in listData" :key="index" :data-index="index" @longpress="long" >
			<!-- 显示撤回消息 -->
			<view v-if="item.iswithdraw" class="flex align-center justify-center pb-4 pt-1">
				<text class="font-sm text-light-muted">你撤回了一条消息</text>
			</view>
			<!-- 显示时间 -->
			<view class="flex align-center justify-center pb-4 pt-2" v-if="showTime(item,index)">
				<text class="font-sm text-light-muted">{{showTime(item,index)}}</text>
			</view>
			<!-- 好友 -->
			<view ref="chatItem"  class="flex align-start justify-start position-relative mb-3" v-if="item.user_id !== 1 && !item.isdelect">
				<avater size="70" :src="item.avatar"></avater>
				<text class="iconfont text-white font-md position-absolute chat-left-icon">&#xe609;</text>
				<div style="max-width:500rpx;" class="bg-white p-2 rounded ml-3">
					<text class="font-md">{{item.data}}</text>
				</div>
			</view>
			<!-- 本人 -->
			<view  ref="chatItem" class="flex align-start justify-end position-relative mb-3" v-if="item.user_id == 1 && !item.iswithdraw && !item.isdelect">
				<div style="max-width:500rpx;" class="p-2 rounded mr-3" :class="{'bg-chat-item':item.type=='text'}">
					<!-- 发送文字 -->
					<text v-if="item.type=='text'" class="font-md">{{item.data}}</text>
					<!-- 发送图片和表情 -->
					<image :lazy-load="true" style="width: 300rpx; height: 300rpx;" v-if="item.type==='image'|| item.type==='emoticon'" :src="item.data" mode="widthFix"></image>
				</div>
				<text class="iconfont text-chat-icon-item font-md position-absolute chat-right-icon" v-if="item.type=='text'">&#xe640;</text>
				<avater size="70" :src="item.avatar"></avater>
			</view>
		</div>
		<!-- 弹出层 -->
		<popup ref="extend" :bodyWidth="300" :bodyHeight="getMenusHeight" bodyBgColor="bg-dark" :tabbarHeight="105" transformOrigin="center center">
			<view class="flex flex-column" style="width: 240rpx;" :style="getMenusStyle">
				<view class="flex-1 flex align-center" hover-class="bg-light" v-for="(item,index) in menus" :key="index" @click="clickEvent(item.event)">
					<text class="font-md pl-3 text-white">{{item.name}}</text>
				</view>
			</view>
		</popup>
	</scroll-view>
</template>

<script>
	// #ifdef APP-PLUS-NVUE
		const dom = weex.requireModule('dom');
	// #endif
	import avater from "./avater.vue"
	import $Time from "@/common/free-lib/time.js"
	import popup from "./popup.vue";
	export default {
		components: {
			avater,
			popup
		},	
		computed: {
			// 动态获取菜单高度
			getMenusHeight() {
				let H = 100
				return this.menus.length * H
			},
			// 获取菜单的样式
			getMenusStyle() {
				return `height: ${this.getMenusHeight}rpx;`
			}
		},
		watch:{
			chartBodyBottom(val,oldVal){
				this.pageToBottom()
			}
		},
		props:{
			listData:{
				type:Array,
				default:()=>[]
			},
			chartBodyBottom:{
				type:String,
				type:''
			}
		},
		data(){
			return {
				propIndex: -1,
				showBoxShaw: false,
				menus: [{
						name: "复制",
						event: "copy"
					},
					{
						name: "发送给朋友",
						event: "seed"
					},
					{
						name: "收藏",
						event: "favorites"
					},
					{
						name: "多选",
						event: "checkbox"
					},
					{
						name: "翻译",
						event: "translation"
					},
					{
						name: "删除",
						event: "delect"
					},
					{
						name: "撤回",
						event: "withdraw"
					}
				],
			}
		},
		methods:{
			// 回到聊天底部
			pageToBottom() {
				// #ifdef APP-PLUS-NVUE
				let chatItem = this.$refs.chatItem;
				let lastIndex = chatItem.length>0?chatItem.length - 1:0;
				if(chatItem[lastIndex]) {
					dom.scrollToElement(chatItem[lastIndex],{})
				}
				// #endif
			},
			// 显示时间
			showTime(item,index){
				let v1 = item.create_time;
				let v2 = index>0?this.listData[index-1].create_time:0
				return $Time.getChatTime(v1,v2);
			},
			long(e) {
				let x = 0
				let y = 0
				// #ifdef APP-PLUS-NVUE
				if (Array.isArray(e.changedTouches) && e.changedTouches.length > 0) {
					x = e.changedTouches[0].screenX
					y = e.changedTouches[0].screenY
				}
				// #endif
				// #ifdef MP
				x = e.detail.x
				y = e.detail.y
				// #endif
				this.propIndex = e.target.attr.dataIndex;
				if(this.listData[e.target.attr.dataIndex].user_id !== 1) {
					this.menus.splice(6,1)
				}else{
					if(this.menus.length==6){
						this.menus.push({
							name: "撤回",
							event: "withdraw"
						})
					}
				}
				this.$refs.extend.show(x, y)
			},
			// 分发菜单事件
			clickEvent(event) {
				switch (event) {
					case "copy":
						break;
					case "seed": 
						break;
					case "favorites":
						break;
					case "checkbox": 
						break;
					case "translation":
						break;
					case "delect": 
						this.$emit('delect',this.propIndex)
						break;
					case "withdraw":
						this.$emit('withdraw',this.propIndex)
						break;
				}
				this.$refs.extend.hide()
			},
		}
	}
</script>

<style>
	.chat-left-icon {
		left: 80rpx;
		top: 20rpx;
	}
	.chat-right-icon {
		right: 80rpx;
		top: 20rpx;
	}
</style>
