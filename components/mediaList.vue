<template>
	<view hover-class="bg-light">
		<div class="flex align-stretch" :class="backClass" @click="onClick" @longpress="long">
			<view class="flex align-center justify-center position-relative" style="width: 145rpx;">
				<wx-avater :src="item.avatar" size="92"></wx-avater>
				<wx-badge badgeClass="position-absolute" badgeStyle="top:15rpx;right:15rpx" v-if="item.noreadnum" :badgeNumber="item.noreadnum"></wx-badge>
			</view>
			<view class="flex flex-column border-bottom flex-1 py-3 pr-3 border-light-secondary">
				<view class="flex align-center justify-between mb-1">
					<text class="font-md">{{item.nickname}}</text>
					<text class="font-sm text-light-muted">{{item.update_time|formatTime}}</text>
				</view>
				<text class="font text-ellipsis text-light-muted">{{item.data}}</text>
			</view>
		</div>
	</view>
</template>

<script>
	import freeBase from "@/common/mixin/free-base.js"
	import Avater from "./avater.vue"
	import Badge from "./badge.vue"
	export default {
		mixins: [freeBase],
		components: {
			"wx-avater":Avater,
			"wx-badge":Badge
		},
		props: {
			item: Object,
			index: Number,
			backClass: String
		},
		methods: {
			onClick(){
				uni.navigateTo({
					url: '/pages/chat/chat',
				});
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
				this.$emit('long',{x,y,index:this.index})
			}
		}
	}
</script>

<style>
</style>
