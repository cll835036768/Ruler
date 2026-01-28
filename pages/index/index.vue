<template>
	<view class="content">
		<view class="desc">
			可以传参的值有默认数值，数值的颜色，刻度尺的颜色，并返回change 事件
		</view>
	<cll-ruler
		  :defaultValue="68"
		  unit="kg"
		  color="#3bd7d0"
		  scaleColor="#cccccc"
		  @ruleChange="ruleChange"
		/>
<cll-ruler
		  :defaultValue="65"
		  unit="cm"
		  color="#ff765f"
		  scaleColor="#FF0000"
		  @ruleChange="ruleChange"
		/>
	</view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const defaultValue = ref(50)

const ruleChange = (e: number) => {
	console.log(e)
}

console.log(124)
</script>

<style scoped lang="scss">
.content {
	display: grid;
	grid-template-columns: 1fr;
	grid-auto-rows: auto;
	gap: 10px;
	justify-content: center;
	.desc{
		width: 100%;
		padding: 10rpx 20rpx;
		
	}
}
</style>