<!-- 静态数据演示版本 适合任何后端 -->
<template>
	<view class="app">
		<button @click="skuKey = true">打开SKU组件</button>
		
		<vk-u-goods-sku-popup
			v-model="skuKey" 
			border-radius="20" 
			:custom-action="findGoodsInfo"
			:mode="skuMode"
			@open="openSkuPopup"
			@close="closeSkuPopup"
			@add-cart="addCart"
			@buy-now="buyNow"
		></vk-u-goods-sku-popup>
		
	</view>
</template>

<script>
	var that;											// 当前页面对象
	var app = getApp();						// 可获取全局配置
	const goodsData1 = {
		"_id":"001",
		"name": "iphone11",
		"goods_thumb":"https://img14.360buyimg.com/n0/jfs/t1/59022/28/10293/141808/5d78088fEf6e7862d/68836f52ffaaad96.jpg",
		"sku_list": [
			{
				"_id": "001",
				"goods_id": "001",
				"goods_name": "iphone11",
				"image": "https://img14.360buyimg.com/n0/jfs/t1/79668/22/9987/159271/5d780915Ebf9bf3f4/6a1b2703a9ed8737.jpg",
				"price": 19800,
				"sku_name": "红色,128G,公开版",
				"sku_name_arr": ["红色", "128G", "公开版"],
				"stock": 100
			},
			{
				"_id": "002",
				"goods_id": "001",
				"goods_name": "iphone11",
				"image": "https://img14.360buyimg.com/n0/jfs/t1/52252/35/10516/124064/5d7808e0E46202391/7100f3733a1c1f00.jpg",
				"price": 9800,
				"sku_name": "白色,256G,公开版",
				"sku_name_arr": ["白色", "256G","公开版"],
				"stock": 100
			},
			{
				"_id": "003",
				"goods_id": "001",
				"goods_name": "iphone11",
				"image": "https://img14.360buyimg.com/n0/jfs/t1/79668/22/9987/159271/5d780915Ebf9bf3f4/6a1b2703a9ed8737.jpg",
				"price": 19800,
				"sku_name": "红色,256G,公开版",
				"sku_name_arr": ["红色","256G","公开版"],
				"stock": 100
			}
		],
		"spec_list": [
			{
				"name": "颜色",
				"list": [
					{
						"name": "红色"
					},
					{
						"name": "黑色"
					},
					{
						"name": "白色"
					}
				]
			},
			{
				"name": "内存",
				"list": [
					{
						"name": "128G"
					},
					{
						"name": "256G"
					}
				]
			},
			{
				"name": "版本",
				"list": [
					{
						"name": "公开版"
					},
					{
						"name": "非公开版"
					}
				]
			}
		]
	};
	const goodsData2 = {
			"_id":"002",
		  "name": "迪奥香水",
			"goods_thumb":"https://res.lancome.com.cn/resources/2020/9/11/15998112890781924_920X920.jpg?version=20200917220352530",
		  "sku_list": [
		    {
		      "_id": "006",
		      "goods_id": "002",
		      "goods_name": "迪奥香水",
		      "image": "https://res.lancome.com.cn/resources/2020/9/11/15998112890781924_920X920.jpg?version=20200917220352530",
		      "price": 19800,
		      "sku_name": "默认",
		      "sku_name_arr": ["默认"],
		      "stock": 100
		    }
		  ],
		  "spec_list": [
		    {
		      "list": [
		        {
		          "name": "默认"
		        }
		      ],
		      "name": "默认"
		    }
		  ]
		};
	export default {
		data() {
			return {
				skuKey:false,
				skuMode:1
			}
		},
		// 监听 - 页面每次【加载时】执行(如：前进)
		onLoad(options) {
			that = this;
			that.init(options);
		},
		methods: {
			// 初始化
			init(options = {}){
				
			},
			// sku组件 开始-----------------------------------------------------------
			openSkuPopup(){
				console.log("监听 - 打开sku组件");
			},
			closeSkuPopup(){
				console.log("监听 - 关闭sku组件");
			},
			// 加入购物车前的判断
			addCartFn(obj){
				let { selectShop } = obj;
				// 模拟添加到购物车,请替换成你自己的添加到购物车逻辑
				let res = {};
				let name = selectShop.goods_name;
				if(selectShop.sku_name != "默认"){
					name += "-"+selectShop.sku_name;
				}
				res.msg = `${name} 已添加到购物车`;
				if(typeof obj.success == "function") obj.success(res);
			},
			// 加入购物车按钮
			addCart(selectShop){
				console.log("监听 - 加入购物车");
				that.addCartFn({
					selectShop : selectShop,
					success : function(res){
						// 实际业务时,请替换自己的加入购物车逻辑
						that.toast(res.msg);
					}
				});
			},
			// 立即购买
			buyNow(selectShop){
				console.log("监听 - 立即购买");
				that.addCartFn({
					selectShop : selectShop,
					success : function(res){
						// 实际业务时,请替换自己的立即购买逻辑
						that.toast("立即购买");
					}
				});
			},
			/**
			 * 获取商品信息
			 * 这里可以看到每次打开SKU都会去重新请求商品信息,为的是每次打开SKU组件可以实时看到剩余库存
			 */
			findGoodsInfo(){
				return new Promise(function (resolve, reject) {
					// 此处写接口请求，并将返回的数据进行处理成goodsData1的数据格式，
					// 并执行resolve
					let goodsInfo = goodsData1;			// 有sku的商品
					//let goodsInfo = goodsData2;   // 无sku的商品
					resolve(goodsInfo);
					
				});
			},
			toast(msg){
				uni.showToast({
					title: msg,
					icon:"none"
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.app {
		padding: 30rpx;
		font-size: 28rpx;
	}
</style>
