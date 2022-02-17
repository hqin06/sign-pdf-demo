<template>
	<view class="content">
		<view style="display: flex;justify-content: flex-start;">
			<button type="primary" @click="sign" class="sign-btn">点击签字</button>
		</view>
	</view>
</template>

<script>
	export default {
		methods: {
			async sign() {
				let pdfFildId = ''
				let signImgId = '';
				let projectId = '';
				uni.navigateTo({
					url: `./SignPdf?pdfFildId=${pdfFildId}&signImgId=${signImgId}&projectId=${projectId}`
				});
			},
		}
	}
</script>

<style>
</style>
