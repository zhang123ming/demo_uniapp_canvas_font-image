<template>
	<view class='page'>
		<input type="text" value="" class="input" v-model="text" placeholder="请输入需要添加的文字" maxlength="5" />
		<button type="primary" @click='copyFn'>生成图片</button>
		<canvas canvas-id="myCanvas" id='sss'></canvas>
		<image :src="base64" mode="" v-if="base64" class='immm'></image>
		<button type="primary" @click='bcFn' v-if="base64">保存图片</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				text: '',
				base64: null
			}
		},
		mounted() {
			this.copyFn();
		},
		methods: {
			copyFn() {
				let ww, hh;
				const query = uni.createSelectorQuery().in(this);
				query.select('#sss').boundingClientRect(data => { //获取canvas-dom
					ww = data.width; //准确的宽高
					hh = data.height
					var ctx = uni.createCanvasContext('myCanvas') //绑定画布
					ctx.drawImage('../../static/2.jpg', 0, 0, ww, hh); //填充进图片
					ctx.setFillStyle('#000') //设置内容1的文字样式
					ctx.setFontSize(30);
					ctx.setTextAlign('center') //设置对于坐标点的对齐方式
					ctx.fillText(this.text, ww / 2, hh / 2 + 55) //计算距离，将文字定位于图片的某处
					ctx.setFillStyle('white') //设置内容2的文字样式
					ctx.setTextAlign('left') //同上
					ctx.setFontSize(16);
					// ctx.fillText('13256789903', ww / 2, hh / 2 + 80)
					// ctx.setFontSize(18);
					// ctx.setTextAlign('right') //同上
					// ctx.fillText('风清云淡', ww / 2, hh / 2 + 80)
					ctx.fillText('13256789903', ww / 2, hh / 2 + 80)
					ctx.setFontSize(18);
					ctx.setTextAlign('right') //同上
					ctx.fillText('风清云淡', 150, hh / 2 + 80)
					ctx.draw(); //输出到画布中
					uni.showLoading({ //增加loading等待效果
						mask: true
					})
					setTimeout(() => { //不加延迟的话，base64有时候会赋予undefined
						uni.canvasToTempFilePath({
							canvasId: 'myCanvas',
							success: (res) => {
								// console.log('==============', res.tempFilePath);
								this.base64 = res.tempFilePath
							}
						})
						uni.hideLoading();
					}, 1200)
				}).exec();

			},
			bcFn() {
				// #ifdef H5
				this.downloadImgByBase64(this.base64);
				// #endif
				// #ifdef APP-PLUS
				uni.saveImageToPhotosAlbum({ //保存图片
					filePath: this.base64,
					success: (res) => {
						uni.showToast({
							title: '保存成功',
						})
					}
				})
				// #endif
			},
			downloadImgByBase64(url) {
				var img = new Image()
				img.onload = function() {
					var canvas = document.createElement('canvas')
					canvas.width = img.width
					canvas.height = img.height
					var ctx = canvas.getContext('2d')
					// 将img中的内容画到画布上
					ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
					// 将画布内容转换为base64
					var base64 = canvas.toDataURL()
					// 创建a链接
					var a = document.createElement('a')
					a.href = base64
					a.download = ''
					// 触发a链接点击事件，浏览器开始下载文件
					a.click()
				}
				img.src = url
				// 必须设置，否则canvas中的内容无法转换为base64
				img.setAttribute('crossOrigin', 'Anonymous')
			}


		}
	}
</script>

<style>
	.input {
		width: 100%;
		height: 80rpx;
		background: #ccc;
	}

	#sss {
		position: absolute;
		width: 750rpx;
		height: 426rpx;
		top: -99999899rpx;
		left: -99999899rpx;
		z-index: 9999;
	}

	.immm {
		width: 750rpx;
		height: 426rpx;
	}
</style>
