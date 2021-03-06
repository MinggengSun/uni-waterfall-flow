# 基于 uni-app 实现的瀑布流组件
对于 image 组件里的 widthFix  模式，会有一个再次加载的问题，会使标签的高度获取不精准，使定位出现偏差。

如果想使用 widthFix 这个模式，最好是后台在回数据的时候，也把图片的尺寸加上，然后通过  :style 给上高度。

如果是利用 uni.createSelectorQuery().selectAll().fields ()  获取到图片信息，是要再多一层循环。

## 兼容平台

H5、APP、小程序。

# HTML 中使用

```方法
<template>
	<view class="app">
		<waterfall-flow :list="list"></waterfall-flow>
	</view>
</template>
```

```
<script>
	// 瀑布流组件
	import WaterfallFlow from '@/components/waterfall-flow.vue';
	// 模拟 JSON 数据
	var data = require('@/common/json/data.json');
	export default {
		data() {
			return {
				page: 1,
				start: 0,
				end: 0,
				list: [] // 列表
			}
		},
		mounted() {
			this.getList();
		},
		// 触底加载更多
		onReachBottom() {
			this.page++;
			this.getList();
		},
		methods: {
			// 模拟数据加载
			getList() {
				if (this.list.length < data.list.length) {
					uni.showLoading({
						title: '加载中'
					});
					setTimeout(() => {
						this.end = this.page * 10;
						this.list = this.list.concat(data.list.slice(this.start, this.end));
						this.start = this.end;
						uni.hideLoading();
					}, 300)
				}
			}
		},
		components: {
			WaterfallFlow
		}
	}
</script>
```

注 ：我们只需要维护 list 数组的拼接就行了。