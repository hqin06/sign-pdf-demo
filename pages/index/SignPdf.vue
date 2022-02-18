<template>
	<view class="content">
		<view>
			<web-view :src="websrc" @message="handleMessage"></web-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				websrc: '',
				appUrl: '/hybrid/html/readPdf/index/index.html', //app内的web地址
				pdfUrl: {
					url: './my.pdf'
				},
				signImgUrl: {
					url: './sign.jpeg'
				},
				title: '合同签名', //pdf文件名称
			}
		},
		onLoad(query) {
			this.refresh(query)
		},
		methods: {
			async refresh(query) {
				try {
					uni.showLoading({
						title: '加载中'
					})
					//app 直接跳转到app内的web页面
					//#ifdef APP-PLUS
					this.websrc = this.appUrl + '?url=' + encodeURIComponent(this.pdfUrl.url) + '&tname=' +
						encodeURIComponent(
							this.title) + '&signImgUrl=' +
						encodeURIComponent(
							this.signImgUrl.url)
					//#endif
				} catch (e) {
					uni.showModal({
						title: '发生错误',
						content: e
					})
				} finally {
					uni.hideLoading()
				}
			},
			async handleMessage(evt) {
				console.log(evt.detail.data, '小程序接受到的消息');
				if (evt.detail.data[0]?.type === 'reSign') {
					uni.navigateTo({
						url: `sign`
					});
				} else {
					uni.navigateBack({
						delta: 1
					});
				}
			}
		}

	}
</script>

<style>

</style>
