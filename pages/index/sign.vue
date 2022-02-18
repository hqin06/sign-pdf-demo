<template>
	<view>
		<view class="wrapper">
			<view class="handBtn">
				<button @click="cancel" class="cancelBtn">取消</button>
				<button @click="saveCanvasAsImg" class="saveBtn">保存到相册</button>
				<button @click="retDraw" class="delBtn">重写</button>
				<button @click="subCanvas" class="subBtn">完成</button>
			</view>
			<view class="handCenter">
				<canvas class="handWriting" :disable-scroll="true" @touchstart="uploadScaleStart"
					@touchmove="uploadScaleMove" canvas-id="handWriting"></canvas>
			</view>
			<view class="handRight">
				<view class="handTitle">请签名</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				canvasName: 'handWriting',
				ctx: '',
				startX: null,
				startY: null,
				canvasWidth: 0,
				canvasHeight: 0,
				selectColor: 'black',
				lineColor: '#1A1A1A', // 颜色
				lineSize: 5, // 笔记倍数
			};
		},
		onLoad() {
			this.ctx = uni.createCanvasContext("handWriting");
			this.$nextTick(() => {
				uni.createSelectorQuery().select('.handCenter').boundingClientRect(rect => {
						this.canvasWidth = rect.width;
						this.canvasHeight = rect.height;
						/* 将canvas背景设置为 白底，不设置  导出的canvas的背景为透明 */
						this.setCanvasBg('#e6e5e5');
					})
					.exec();
			});
		},
		methods: {
			cancel() {
				uni.navigateBack({
					delta: 1
				});
			},
			// 笔迹开始
			uploadScaleStart(e) {
				this.startX = e.changedTouches[0].x
				this.startY = e.changedTouches[0].y
				//设置画笔参数
				//画笔颜色
				this.ctx.setStrokeStyle(this.lineColor)
				//设置线条粗细
				this.ctx.setLineWidth(this.lineSize)
				//设置线条的结束端点样式
				this.ctx.setLineCap("round") //'butt'、'round'、'square'
				//开始画笔
				this.ctx.beginPath()
			},
			// 笔迹移动
			uploadScaleMove(e) {
				//取点
				let temX = e.changedTouches[0].x
				let temY = e.changedTouches[0].y
				//画线条
				this.ctx.moveTo(this.startX, this.startY)
				this.ctx.lineTo(temX, temY)
				this.ctx.stroke()
				this.startX = temX
				this.startY = temY
				this.ctx.draw(true)
			},
			/**
			 * 重写
			 */
			retDraw() {
				this.ctx.clearRect(0, 0, 700, 730);
				this.ctx.draw();
				//设置canvas背景
				this.setCanvasBg('#e6e5e5');
			},
			//完成
			subCanvas() {
				uni.canvasToTempFilePath({
					canvasId: 'handWriting',
					fileType: 'png',
					quality: 1, //图片质量
					success(res) {
						// console.log(res.tempFilePath, 'canvas生成图片地址');
					
						
					}
				});
			},
			//保存到相册
			saveCanvasAsImg() {
				uni.canvasToTempFilePath({
					canvasId: 'handWriting',
					fileType: 'png',
					quality: 1, //图片质量
					success(res) {
						console.log(res.tempFilePath, 'canvas生成图片地址');
						uni.saveImageToPhotosAlbum({
							filePath: res.tempFilePath,
							success(res) {
								uni.showToast({
									title: '已保存到相册',
									duration: 2000
								});
							}
						});
					}
				});
			},
			//预览
			previewCanvasImg() {
				uni.canvasToTempFilePath({
					canvasId: 'handWriting',
					fileType: 'jpg',
					quality: 1, //图片质量
					success(res) {
						uni.previewImage({
							urls: [res.tempFilePath] //预览图片 数组
						});
					}
				});
			},
			//设置canvas背景色  不设置  导出的canvas的背景为透明
			//@params：字符串  color
			setCanvasBg(color) {

				/* 将canvas背景设置为 白底，不设置  导出的canvas的背景为透明 */
				//rect() 参数说明  矩形路径左上角的横坐标，左上角的纵坐标, 矩形路径的宽度, 矩形路径的高度
				//这里是 canvasHeight - 4 是因为下边盖住边框了，所以手动减了写
				this.ctx.rect(0, 0, this.canvasWidth, this.canvasHeight - 4);
				// ctx.setFillStyle('red')
				this.ctx.setFillStyle(color);
				this.ctx.fill(); //设置填充
				this.ctx.draw(); //开画
			}
		}
	};
</script>

<style scoped>
	page {
		background: #fbfbfb;
		height: auto;
		overflow: hidden;
	}

	.wrapper {
		width: 100%;
		height: 90vh;
		margin: 100rpx 0 30rpx 0;
		overflow: hidden;
		display: flex;
		align-content: center;
		flex-direction: row;
		justify-content: center;
		font-size: 28rpx;
	}

	.handWriting {
		background: #fff;
		width: 100%;
		height: 95vh;
	}

	.handRight {
		display: inline-flex;
		align-items: center;
	}

	.handCenter {
		border: 4rpx dashed #e9e9e9;
		flex: 5;
		overflow: hidden;
		box-sizing: border-box;
	}

	.handTitle {
		transform: rotate(90deg);
		flex: 1;
		color: #333;
		font-weight: 900;
		font-size: 34rpx;
	}

	.handBtn button {
		font-size: 28rpx;
	}

	.handBtn {
		height: 95vh;
		display: inline-flex;
		flex-direction: column;
		justify-content: space-between;
		align-content: space-between;
		flex: 1;
	}

	.cancelBtn {
		position: absolute;
		top: 120rpx;
		left: 0rpx;
		transform: rotate(90deg);
		color: #666;
	}

	.delBtn {
		position: absolute;
		bottom: 220rpx;
		left: -3rpx;
		transform: rotate(90deg);
		color: #e22d2d;
	}

	.saveBtn {
		position: absolute;
		bottom: 480rpx;
		left: 56rpx;
		transform: rotate(90deg);
		transform-origin: left;
		color: #666;
	}

	.subBtn {
		position: absolute;
		bottom: 52rpx;
		left: -3rpx;
		display: inline-flex;
		transform: rotate(90deg);
		background: #008ef6;
		color: #fff;
		margin-bottom: 30rpx;
		text-align: center;
		justify-content: center;
	}
</style>
