<template>
	<view class="test">

		<text class="test-text">
			{{content}}
		</text>

		<view class="test-show" :style="style">
			<view class="test-view" :style="{backgroundColor:item.color}" v-for="(item,index) in list" :key='index'>
				{{item.title}}
			</view>
		</view>

		<view class="test-title">
			滑动改变item数量
		</view>
		<view class="test-slider">
			<slider show-value value="list.length" @change="sliderChange" @changing="sliderChange" step="1"
				activeColor='#007AFF' min="1" max="3" block-size="20" />
		</view>

		<view class="test-title">
			布局方向
		</view>

		<view class="test-tag">
			<template v-if="directions">
				<view v-for="(item,tagindex) in directions" :key="tagindex" class="tag"
					@click="didClickDirectionItems(item)" :class="{active:item.seletecd}">
					<view class="text">
						{{item.title}}
					</view>
				</view>
			</template>
		</view>

		<view class="test-title">
			主轴对齐方式
		</view>
		<view class="test-tag">
			<template v-if="alignsColumn">
				<view v-for="(item,tagindex) in alignsColumn" :key="tagindex" class="tag"
					@click="didClickAlignColumnItems(item)" :class="{active:item.seletecd}">
					<view class="text">
						{{item.title}}
					</view>
				</view>
			</template>
		</view>
		<view class="test-title">
			交叉肉对齐方式
		</view>

		<view class="test-tag">
			<template v-if="alignsRow">
				<view v-for="(item,tagindex) in alignsRow" :key="tagindex" class="tag"
					@click="didClickAlignRowItems(item)" :class="{active:item.seletecd}">
					<view class="text">
						{{item.title}}
					</view>
				</view>
			</template>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				directions: [],
				alignsRow: [],
				alignsColumn: [],
				style: '',
				content: '',
				list: [],
				height: 0,
			};
		},

		onLoad() {
			this.initData();
		},

		methods: {

			color16() {
				return "#" + Math.random().toString(16).slice(-6);
			},

			sliderChange(e) {
				let count = e.detail.value;
				let number = this.list.length;
				if (count > number) {
					let c = count - number;
					for (let i = 0; i < c; i++) {
						let map = {
							'title': this.list.length + 1,
							'color': this.color16(),
							'height': (i + 1) * 10,
							'width': (i + 1) * 10,
						}
						this.list.push(map);
					}
				} else {
					let c = number - count;
					this.list.splice(0, c);
					for (let i = 0; i < this.list.length; i++) {
						let map = this.list[i];
						map.title = i + 1;
					}
				}
				this.setCustomStyle();
			},

			// 设置样式
			setCustomStyle() {
				let directionMap = this.directions.filter(item => item.seletecd)[0];
				let rowMap = this.alignsRow.filter(item => item.seletecd)[0];
				let columnMap = this.alignsColumn.filter(item => item.seletecd)[0];
				this.style = "flex-direction: " + directionMap.title + ';' + "align-items: " + rowMap.title +
					';' + "justify-content: " + columnMap.title + ';';
				this.content = "flex-direction: " + directionMap.details + "\n" + "align-items: " + rowMap.details + '\n' +
					"justify-content: " + columnMap.details + '\n';
			},

			didClickDirectionItems(item) {
				this.directions.forEach(aItem => {
					aItem.seletecd = false;
				});
				item.seletecd = !item.seletecd;
				this.setCustomStyle();
			},

			didClickAlignRowItems(item) {
				this.alignsRow.forEach(aItem => {
					aItem.seletecd = false;
				});
				item.seletecd = !item.seletecd;
				this.setCustomStyle();
			},

			didClickAlignColumnItems(item) {
				this.alignsColumn.forEach(aItem => {
					aItem.seletecd = false;
				});
				item.seletecd = !item.seletecd;
				this.setCustomStyle();
			},

			initData() {
				this.directions = [{
						'title': 'row',
						'seletecd': true,
						'details': '横向布局',
					},
					{
						'title': 'column',
						'seletecd': false,
						'details': '竖直布局',
					},
				];

				this.alignsColumn = [{
						'title': 'flex-start',
						'seletecd': true,
						'details': '主轴从开始位置开始排列'
					},
					{
						'title': 'flex-end',
						'seletecd': false,
						'details': '主轴从结束位置开始排列'
					},
					{
						'title': 'center',
						'seletecd': false,
						'details': '主轴居中排列'
					},
					{
						'title': 'space-between',
						'seletecd': false,
						'details': '主轴两头对齐等间距排列'
					},
					{
						'title': 'space-around',
						'seletecd': false
					},
					// {
					// 	'title': 'stretch',
					// 	'seletecd': false
					// },
				];

				this.alignsRow = [{
						'title': 'flex-start',
						'seletecd': true,
						'details': '交叉轴从开始位置开始排列'
					},
					{
						'title': 'flex-end',
						'seletecd': false,
						'details': '交叉轴从结束位置开始排列'
					},
					{
						'title': 'center',
						'seletecd': false
					},
					// {
					// 	'title': 'baseline',
					// 	'seletecd': false
					// },
					// {
					// 	'title': 'stretch',
					// 	'seletecd': false
					// },
				];
				for (let i = 0; i < 1; i++) {
					let map = {
						'title': this.list.length + 1,
						'color': this.color16(),
						'height': (i + 1) * 10,
						'width': (i + 1) * 10,
					}
					this.list.push(map);
				}
				this.setCustomStyle();
			},
		}
	};
</script>

<style lang="scss">
	page {
		height: 100%;
		width: 100%;
		background-color: #FFFFFF;
	}

	.test-slider {
		width: 100%;
		margin-bottom: 20rpx;
	}

	.test-text {
		color: #333333;
		font-size: 24rpx;
		margin-bottom: 20rpx;
	}

	.test-show {
		background-color: #FFFFFF;
		display: flex;
		margin-bottom: 20rpx;
		height: 240rpx;
	}

	.test-tag {
		display: flex;
		flex-wrap: wrap;
		margin-top: 10rpx;

		.tag {
			height: 50rpx;
			border-radius: 15rpx;
			margin-right: 20rpx;
			margin-bottom: 20rpx;
			background-color: #FFFFFF;
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: center;
			border: 1px solid #d5d5d5;
			padding-left: 10rpx;
			padding-right: 10rpx;

			&.active {
				background-color: #007AFF;

				.text {
					color: #FFFFFF;
				}
			}

			.text {
				line-height: 50rpx;
				color: #666666;
				font-size: 24rpx;
			}
		}
	}

	.test {
		border-radius: 20rpx;
		background-color: #F7F8FA;
		display: flex;
		flex-direction: column;
		margin: 20rpx;
		padding: 20rpx;

		.test-title {
			font-size: 28rpx;
			color: #333333;
			font-weight: 600;
			margin-bottom: 10rpx;
		}

		.test-view {
			height: 50rpx;
			width: 50rpx;
			font-size: 34rpx;
			font-weight: 600;
			line-height: 50rpx;
			text-align: center;
			color: #FFFFFF;
			margin-right: 20rpx;
			margin-bottom: 20rpx;
		}
	}
</style>
