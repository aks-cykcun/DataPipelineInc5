<template>
	<view class="app">
		<button @click="sku_key = true">打开SKU组件</button>
		<vk-u-goods-sku-popup
			v-model="sku_key" 
			border-radius="20" 
			:custom-action="findGoodsInfo"
			:mode="form.skuMode"
			:buy-now-text="form.buyNowText"
			:buy-now-color = "form.buyNowColor"
			:buy-now-background-color="form.buyNowBackgroundColor"
			:add-cart-text="form.addCartText"
			:add-cart-color = "form.addCartColor"
			:add-cart-background-color="form.addCartBackgroundColor"
			:no-stock-text="form.noStockText"
			:min-buy-num="form.minBuyNum"
			:max-buy-num="form.maxBuyNum"
			:step-buy-num="form.stepBuyNum"
			:show-close="form.showClose"
			:mask-close-able="form.maskCloseAble"
			:price-color="form.priceColor"
			@open="openSkuPopup"
			@close="closeSkuPopup"
			@add-cart="addCart"
			@buy-now="buyNow"
		></vk-u-goods-sku-popup>
		<view class="config-wrap">
			<view class="config-title">
				参数配置
			</view>
			<view>
				<view class="form-item">
					<view class="title" style="width: 180rpx;">更换商品</view>
					<radio-group name="radio"  @change="goodsChange">
						<view class="radio">
							<radio value="001" checked/><text>商品1：多组多规格商品</text>
						</view>
						<view class="radio">
							<radio value="002" /><text>商品2：单组多规格商品</text>
						</view class="radio">
						<view class="radio">
							<radio value="003" /><text>商品3：单组单规格商品</text>
						</view>
						<view class="radio">
							<radio value="004" /><text>商品4：暂无库存商品</text>
						</view>
					</radio-group>
				</view>
				
				<view class="form-item">
					<view class="title" style="width: 180rpx;">模式</view>
					<radio-group name="radio"  @change="skuModeChange">
						<view class="radio">
							<radio value="1" checked/><text>都显示</text>
						</view>
						<view class="radio">
							<radio value="2" /><text>只显示购物车</text>
						</view class="radio">
						<view class="radio">
							<radio value="3" /><text>只显示立即购买</text>
						</view>
					</radio-group>
				</view>
				<view class="form-item">
					<view class="title">立即购买文字</view>
					<view class="input-view">
						<view style="width: 30rpx;height: 30rpx;"></view>
						<input class="input" v-model="form.buyNowText"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">立即购买文字颜色</view>
					<view class="input-view">
						<view :style="'background-color: '+form.buyNowColor+';width: 30rpx;height: 30rpx;'"></view>
						<input class="input" v-model="form.buyNowColor"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">立即购买按钮背景色</view>
					<view class="input-view">
						<view :style="'background-color: '+form.buyNowBackgroundColor+';width: 30rpx;height: 30rpx;'"></view>
						<input class="input" v-model="form.buyNowBackgroundColor"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">加入购物车文字</view>
					<view class="input-view">
						<view style="width: 30rpx;height: 30rpx;"></view>
						<input class="input" v-model="form.addCartText"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">加入购物车文字颜色</view>
					<view class="input-view">
						<view :style="'background-color: '+form.addCartColor+';width: 30rpx;height: 30rpx;'"></view>
						<input class="input" v-model="form.addCartColor"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">加入购物车按钮背景色</view>
					<view class="input-view">
						<view :style="'background-color: '+form.addCartBackgroundColor+';width: 30rpx;height: 30rpx;'"></view>
						<input class="input" v-model="form.addCartBackgroundColor"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">无库存时按钮文字</view>
					<view class="input-view">
						<view style="width: 30rpx;height: 30rpx;"></view>
						<input class="input" v-model="form.noStockText"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">价格的字体颜色</view>
					<view class="input-view">
						<view :style="'background-color: '+form.priceColor+';width: 30rpx;height: 30rpx;'"></view>
						<input class="input" v-model="form.priceColor"/>
					</view>
				</view>
				<view class="form-item">
					<view class="title">最小购买数量</view>
					<vk-u-number-box
						v-model="form.minBuyNum" :min="1" :max="10000" :step="1" :positive-integer="true">
					</vk-u-number-box>
				</view>
				<view class="form-item">
					<view class="title">最大购买数量</view>
					<vk-u-number-box
						v-model="form.maxBuyNum" :min="1" :max="10000" :step="1" :positive-integer="true">
					</vk-u-number-box>
				</view>
				<view class="form-item">
					<view class="title">步进器步长</view>
					<vk-u-number-box
						v-model="form.stepBuyNum" :min="1" :max="10000" :step="1" :positive-integer="true">
					</vk-u-number-box>
				</view>
				<view class="form-item">
					<view class="title">显示关闭按钮</view>
					<switch checked @change="showCloseChange" />
				</view>
				<view class="form-item">
					<view class="title">点击遮罩关闭组件</view>
					<switch checked @change="maskCloseAbleChange" />
				</view>
		</view>
		</view>
	</view>
</template>

<script>
	var that;											// 当前页面对象
	var app = getApp();						// 可获取全局配置
	export default {
		data() {
			return {
				goods_id:"001",
				sku_key:false,
				form:{
					skuMode:1,
					buyNowText:"立即购买",
					buyNowColor:"#ffffff",
					buyNowBackgroundColor:"#fe560a",
					addCartText:"加入购物车",
					addCartColor:"#ffffff",
					addCartBackgroundColor:"#ff9402",
					noStockText:"该商品已抢完",
					minBuyNum:1,
					maxBuyNum:10000,
					stepBuyNum:1,
					priceColor:"#fe560a"
				}
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
					that.callFunction({
						success(data) {
							resolve(data.goodsInfo);
						}
					});
				});
			},
			toast(msg){
				uni.showToast({
				    title: msg,
						icon:"none"
				});
			},
			callFunction(obj){
				uni.showLoading({
					title: '请求中'
				});
				uniCloud.callFunction({
					name: 'findGoodsInfo',
					data: { 
						goods_id : that.goods_id
					},
					success(res){
						console.log(res);
						if(typeof obj.success == "function") obj.success(res.result);
					},
					fail(err){
						console.error(err);
					},
					complete(){
						uni.hideLoading();
					}
				});
			},
			// 参数配置开始 -----------------------------------------------------------
			goodsChange(e){
				that.goods_id = e.detail.value;
				that.sku_key = true;
			},
			showCloseChange(e){
				that.form.showClose = e.detail.value;
				that.sku_key = true;
			},
			maskCloseAbleChange(e){
				that.form.maskCloseAble = e.detail.value;
				that.sku_key = true;
			},
			skuModeChange(e){
				that.form.skuMode = e.detail.value;
				that.sku_key = true;
			}
			// 参数配置结束 -----------------------------------------------------------
		}
	}
</script>

<style lang="scss" scoped>
	.app {
		padding: 30rpx;
		font-size: 28rpx;
	}
	.form-item{
		display: flex;
	}
	.form-item .title, {
		padding: 20rpx 0;
		width:360rpx
	}
	.form-item .input-view {
		display: flex;
		align-items: center;
	}
	.form-item .input {
		margin-left: 40rpx;
		border: 1px solid #d6d6d6;
		border-radius: 10rpx;
		padding: 8rpx 30rpx;
		font-size: 28rpx;
	}
	.radio{
		padding: 6rpx 0rpx;
	}
	.config-title {
		text-align: center;
		font-size: 32rpx;
		font-weight: bold;
		margin-bottom: 40rpx;
		margin-top: 40rpx;
		padding-bottom: 10rpx;
	}
	
</style>
