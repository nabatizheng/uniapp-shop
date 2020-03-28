<template>
	<view class="shop-cart">
		<template v-if=" list.length >0 ">
			
		
		
		<!-- 自定义导航栏 -->
		<uni-nav-bar 
		title="购物车" 
		:right-text=' isNavBar?"完成":"编辑" '
		fixed="true"
		status-bar="true"
		@clickRight=' isNavBar = !isNavBar '
		
		></uni-nav-bar>
		<!-- uniapp 不明bug 要写成这样@clickRight=""-->
		
		<!-- 商品内容 -->
		<view class="shop-list">
			<!-- 商品图片块 -->
			<view class="shop-item" v-for="(item,index) in list" :key="index">
				<label class="radio" @tap='selectedItem(index)'>
					<radio value="" color="#FF3333" :checked="item.checked"/><text></text>
				</label>
				<image class="shop-img" :src="item.imgUrl"></image>
				<!-- 文字块 -->
				<view class="shop-text">
					<view class="shop-name">{{item.name}}</view>
					<view class="shop-color f-color">{{item.color}}</view>
				<view class="shop-price">
					<view>￥{{item.pprice}}</view>
					
					
					<!-- 正常 -->
					<template v-if="!isNavBar">
						<view>*{{item.num}}</view>
					</template>
					<!--如果点击编辑  -->
					
					<template v-else>
						<uniNumberBox
							:value='item.num'
							min='1'
							@change = "changeNumber($event,index)"
							>
						</uniNumberBox>
					</template>
					
					
				</view>
				</view>
			</view>
			
		</view>
		
		<!-- 底部 -->
		<view class="shop-foot">
			<label class="radio foot-radio" @click="checkAllFn">
				<radio value="" color="#FF3333" :checked="checkedAll"/><text>全选</text>
			</label>
			
			<template v-if="!isNavBar">
				<view class="foot-total">
				<view class="foot-count">合计:<text class="f-active-color">¥{{totalCount.pprice}}</text></view>
				<view class="foot-num"
				@tap="goconfirm">结算({{totalCount.num}})</view>
			</view>
			</template>
			
			<template v-else>
			<view class="foot-total">
				<view class="foot-num" style="background-color:black;">移入收藏夹</view>
				<view class="foot-num" @tap='delGoodsFn'>删除</view>
			</view>
			</template>
			
			
		</view>
		</template>
		<template v-else>
			
			<uni-nav-bar title="购物车" fixed="true" status-bar="true"></uni-nav-bar>
			
			<view class="shop-else-list">
				<text>囧~购物车还是空的</text>
			</view>
		</template>
		<Tabbar curreentPage='shopcard'></Tabbar>
	</view>
</template>

<script>
	import uniNumberBox from '../../components/uni/uni-number-box/uni-number-box.vue'
	import {mapState,mapActions,mapGetters,mapMutations} from 'vuex'
	import uniNavBar from '@/components/uni/uni-nav-bar/uni-nav-bar.vue'
	import Tabbar from '../../components/common/Tabbar.vue'
	export default {
		data() {
			return {
				isNavBar:false
				
			}
		},
		computed:{
			...mapState({
				list:state=>state.cart.list
			}),
			...mapGetters(['checkedAll','totalCount'])
		},
		components:{
			uniNavBar,
			uniNumberBox,
			Tabbar
			
		},
		methods: {
			...mapActions(['checkAllFn','delGoodsFn']),
			...mapMutations(['selectedItem']),
			changeNumber(value,index){
				this.list[index].num = value;
			},
			//确认订单
			goconfirm(){
				uni.navigateTo({
					url:"../confirm-order/confirm-order"
				})
			}
			
		}
	}
</script>

<style scoped>
	.shop-list{
		padding-bottom: 100rpx;
	}
	.shop-else-list{
		position: absolute;
		left: 0;
		top: 0;
		right: 0;
		bottom: 0;
		display: flex;
		background-color:#F7F7F7 ;
		width: 100%;
		height: 100%;
		justify-content: center;
		align-items: center;
		
		}
	.shop-item{
		display: flex;
		padding: 20rpx;
		align-items: center;/*垂直居中*/
		background-color: #F7F7F7;
		margin-bottom: 10rpx;
	}
	.shop-img{
		width: 200rpx;
		height: 200rpx;
	}
		
	.shop-text{
		flex: 1;
		padding-left: 20rpx;
	}
	.shop-color{
		font-size: 24rpx;
		
	}
	.shop-price{
		display: flex;
		justify-content: space-between;
	}

	.shop-foot{
		border-top: 2rpx solid #F7F7F7;
		background-color: #FFFFFF;
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		height: 100rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.foot-radio{
		padding-left: 20rpx;
	}
	.foot-total{
		display: flex;
	}
	.foot-count{
	line-height: 100rpx;
	padding: 0 20rpx;
	font-size: 32rpx;
	}
	.foot-num{
		background-color:#49BDFB;
		color:#FFFFFF;
		padding:0 60rpx ;
		line-height: 100rpx;
	}

</style>
