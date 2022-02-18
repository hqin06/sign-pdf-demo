<template>
	<view class="content">
		<view style="margin: 20px;">
			<button type="primary" @click="signPdf" class="sign-btn">pdf签字</button>
		</view>
		<view style="margin: 20px;">
			<button type="primary" @click="sign" class="sign-btn">绘制签名</button>
		</view>
	</view>
</template>

<script>
	export default {
		methods: {
			async signPdf() {
				let pdfFildId = ''
				let signImgId = '';
				let projectId = '';
				uni.navigateTo({
					url: `./SignPdf?pdfFildId=${pdfFildId}&signImgId=${signImgId}`
				});
			},
			async sign() {
				uni.navigateTo({
					url: `./sign`
				});
			},
		}
	}
</script>

<style>
	.content{
	/* 	display: flex;
		flex-direction: column;
		align-items: center;
		width: 100%; */
	}
</style>
