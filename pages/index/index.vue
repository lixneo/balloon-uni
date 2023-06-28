<template>
	<view>
		<canvas canvas-id="canvas" class="canvas" :style="{height:btom}" :start="startStatus"
			:change:start="animate.start" :data-width="canvasWidth" :data-height="canvasHeight"></canvas>
	</view>
</template>

<script module="animate" lang="renderjs">
	export default {
		methods: {
			start(newVal, oldVal, owner, ins) {
				if (newVal) {
					let canvasWidth = ins.getDataset().width,
						canvasHeight = ins.getDataset().height,
						canvasEle = document.querySelectorAll('.canvas>canvas')[0],
						ctx = canvasEle.getContext('2d');

					function animate() {
						ctx.clearRect(0, 0, canvasWidth, canvasHeight)
						ctx.beginPath()
						ctx.moveTo(100, 100)
						ctx.lineTo(200, 200)
						ctx.stroke()
						requestAnimationFrame(animate)
					}
					animate()
				}
			}
		}
	}
</script>

<script>
	export default {
		data() {
			return {
				title: 'canvas',
				canvasWidth: 0,
				canvasHeight: 0,
				startStatus: false,
				ballList: [],
				btom: ''
			}
		},
		onReady: function() {
			this.$nextTick(() => {
				uni.createSelectorQuery().select(".canvas").boundingClientRect(data => {
					this.canvasWidth = data.width
					this.canvasHeight = data.height
					this.startStatus = true
				}).exec()
			})
		},
		onShow() {
			this.btom = uni.getWindowInfo().windowHeight + "px"
		}
	}
</script>

<style>
	.page-body-wrapper {
		text-align: center;
	}

	.canvas {
		width: 100vw;
		margin: auto;
		background-color: #eeeeee;
	}
</style>