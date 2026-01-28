<template>
	<view class="container">
		<view class="ruler-wrapper">
			<!-- 顶部数值 + 指针 -->
			<view class="pointer-box">
				<text class="value-text" :style="{ color: props.color }">
					<text class="value-number" :style="{ color: props.color }">{{ currentNumber }}</text>
					<text class="value-unit">{{ props.unit }}</text>
				</text>

				<image class="arrow-icon" :src="arrow" />

				<view class="center-line" :style="{
            boxShadow: `0 0 2rpx ${props.color}`,
          }" />
			</view>

			<!-- 刻度尺 -->
			<swiper class="ruler-swiper" :duration="10" easing-function="linear" :display-multiple-items="multipleItems"
				:current="current" @change="getCurrent">
				<swiper-item v-for="(item, index) in arr" :key="index" class="swiper-item">
					<view v-if="item !== ''" :class="item % 5 === 0 ? 'scale-line-long' : 'scale-line-short'"
						:style="{ backgroundColor: props.scaleColor }" />
					<view class="scale-text">
						{{ item % 5 === 0 ? item : '' }}
					</view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script lang="ts" setup>
	import { ref, watch, onMounted, onBeforeMount } from 'vue'
	import arrow from './arrow.png'

	/* ================== Props ================== */
	const props = defineProps({
		defaultValue: {
			type: Number,
			default: 50,
		},
		unit: {
			type: String,
			default: 'kg',
		},
		color: {
			type: String,
			default: '#3bd7d0',
		},
		scaleColor: {
			type: String,
			default: '#dfdfdf',
		},
	})

	const emit = defineEmits<{
		(e : 'ruleChange', value : number) : void
	}>()

	/* ================== 配置 ================== */
	const min = 10
	const max = 110
	const offset = 15
	const multipleItems = 31

	/* ================== 状态 ================== */
	const arr = ref<(number | string)[]>([])
	const current = ref(0)
	const currentNumber = ref(props.defaultValue)

	/* ================== 初始化刻度 ================== */
	function initData() {
		const list : (number | string)[] = []
		for (let i = min - offset; i <= max + offset; i++) {
			if (i < min || i > max) {
				list.push('')
			} else {
				list.push(i)
			}
		}
		arr.value = list
	}

	onBeforeMount(() => {
		initData()
		current.value = currentNumber.value - min + offset - Math.floor(multipleItems / 2)
	})

	onMounted(() => {
		emit('ruleChange', currentNumber.value)
	})

	/* ================== 外部值变化 ================== */
	watch(
		() => props.defaultValue,
		(val) => {
			currentNumber.value = val
			current.value =
				val - min + offset - Math.floor(multipleItems / 2)
		}
	)

	/* ================== 滑动取值（核心） ================== */
	function getCurrent(e : any) {
		const leftIndex = e.detail.current

		// 中线刻度 index
		const centerIndex =
			leftIndex + Math.floor(multipleItems / 2)

		// 中线刻度真实值
		const value = min + centerIndex - offset

		if (value < min || value > max) return

		currentNumber.value = value
		emit('ruleChange', value)
	}
</script>

<style scoped>
	.container {
		width: 100%;
		padding: 0 32rpx;
	}

	.ruler-wrapper {
		position: relative;
		height: 165rpx;
	}

	/* 顶部指针 */
	.pointer-box {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		text-align: center;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	/* 数值 */
	.value-number {
		font-family: Oswald;
		font-size: 40rpx;
		font-weight: bold;
		line-height: 54rpx;
	}

	.value-unit {
		font-size: 24rpx;
		margin-left: 4rpx;
	}

	/* 箭头 */
	.arrow-icon {
		width: 28rpx;
		height: 20rpx;
		margin-bottom: 4rpx;
	}

	/* 中线 */
	.center-line {
		width: 4rpx;
		height: 40rpx;
	}

	/* swiper */
	.ruler-swiper {
		width: 100%;
		height: 165rpx;
		padding-top: 74rpx;
		box-sizing: border-box;
	}

	/* 刻度 */
	.swiper-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		height: 120rpx;
		font-size: 24rpx;
		border-top: 1px solid #f5f7f9;
	}

	/* 短刻度 */
	.scale-line-short {
		width: 1px;
		height: 28rpx;
		margin-bottom: 10rpx;
	}

	/* 长刻度 */
	.scale-line-long {
		width: 1px;
		height: 48rpx;
	}

	/* 数字 */
	.scale-text {
		width: 100%;
		text-align: center;
		font-size: 24rpx;
		color: #999999;
	}
</style>