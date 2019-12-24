<template>
	<div>
	<view>
		<!-- 上方导航栏 -->
		<div class="title-bar">
		<view class="cu-bar bg-grey light">
			<view class="action">
				<text class="cuIcon-homefill text-gray"></text>
				首页
			</view>
			<view class="content text-bold">华龙e生活</view>
			<view class="action">
				<text class="cuIcon-cardboardfill text-grey"></text>
				<text class="cuIcon-recordfill text-red"></text>
			</view>
		</view>
		</div>
		<view class="content">
			<swiper class="screen-swiper" :class="dotStyle?'square-dot':'round-dot'" :indicator-dots="true" :circular="true"
			 :autoplay="true" interval="5000" duration="500">
				<swiper-item v-for="(item,index) in swiperList" :key="index">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'"></image>
					<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
				</swiper-item>
			</swiper>
		</view>
		<view class="cu-bar bg-white solid-bottom margin-top">
			<view class="action">
				<text class="cuIcon-title text-orange "></text> 功能列表
			</view>
			<view class="action">
				<button class="cu-btn bg-green shadow" @tap="showModal" data-target="gridModal">设置</button>
			</view>
		</view>
		<view class="cu-modal" :class="modalName=='gridModal'?'show':''" @tap="hideModal">
			<view class="cu-dialog" @tap.stop>
				<radio-group class="block" @change="Gridchange">
					<view class="cu-list menu text-left">
						<view class="cu-item" v-for="(item,index) in 3" :key="index">
							<label class="flex justify-between align-center flex-sub">
								<view class="flex-sub">{{index +3}} 列</view>
								<radio class="round" :value="(index + 3) + ''" :class="gridCol==index+3?'checked':''" :checked="gridCol==index+3"></radio>
							</label>
						</view>
					</view>
				</radio-group>
				<view class="cu-list menu text-left solid-top">
					<view class="cu-item">
						<view class="content">
							<text class="text-grey">边框</text>
						</view>
						<view class="action">
							<switch @change="Gridswitch" :class="gridBorder?'checked':''" :checked="gridBorder?true:false"></switch>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="cu-list grid" :class="['col-' + gridCol,gridBorder?'':'no-border']"> 
			<view class="cu-item" v-for="(item,index) in cuIconList" :key="index" v-if="index<gridCol*3"><!-- 调整显示几行 -->
				<view :class="['cuIcon-' + item.cuIcon,'text-' + item.color]">
					<view class="cu-tag badge" v-if="item.badge!=0">
						<block v-if="item.badge!=1">{{item.badge>99?'99+':item.badge}}</block>
					</view>
				</view>
				<text>{{item.name}}</text>
			</view>
		</view>
		
		
		<view class="cu-bar bg-white solid-bottom margin-top">
			<view class="action">
				<text class="cuIcon-titles text-orange "></text> 通知公告
			</view>
		</view>
		
		<view class="cu-card article" :class="isCard?'no-card':''">
				<view class="cu-item shadow">
					<view class="title"><view class="text-cut">壹号院样板工程即将上线</view></view>
					<view class="content">
						<image src="../../static/ad-001.png"
						 mode="aspectFill"></image>
						<view class="desc">
							<view class="text-content"> 华龙壹号院进入紧张的施工攻坚期，为给业主交房提供最后的完美保障!</view>
							<view>
								<view class="cu-tag bg-red light sm round">通知</view>
								<view class="cu-tag bg-green light sm round">物业</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="cu-card article" :class="isCard?'no-card':''">
				<view class="cu-item shadow">
					<view class="title"><view class="text-cut">在线缴费领红包啦</view></view>
					<view class="content">
						<image src="../../static/ad-004.png"
						 mode="aspectFill"></image>
						<view class="desc">
							<view class="text-content"> 在家即可缴纳电费物业费等费用，免去您来回奔波的麻烦，即可缴费还有红包领取呦！</view>
							<view>
								<view class="cu-tag bg-red light sm round">活动</view>
								<view class="cu-tag bg-green light sm round">财务</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<swiper class="card-swiper" :class="dotStyle?'square-dot':'round-dot'" :indicator-dots="true" :circular="true"
		 :autoplay="true" interval="5000" duration="500" @change="cardSwiper" indicator-color="#8799a3"
		 indicator-active-color="#0081ff">
			<swiper-item v-for="(item,index) in swiperList" :key="index" :class="cardCur==index?'cur':''">
				<view class="swiper-item">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'"></image>
					<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
				</view>
			</swiper-item>
		</swiper>
		
		
		</br>
		</br>
		</br>
		</br>
		</br>
		
		<view class="box">
			
			<view class="cu-bar tabbar bg-white">
				<view class="action">
					<view class="cuIcon-cu-image">
						<image src="/static/tabbar/basics_cur.png"></image>
					</view>
					<view class="text-green">首页</view>
				</view>
				<view class="action">
					<view class="cuIcon-cu-image">
						<image src="/static/tabbar/component.png"></image>
					</view>
					<view class="text-gray">生活驿站</view>
				</view>
				<view class="action">
					<view class="cuIcon-cu-image">
						<image src="/static/tabbar/plugin.png"></image>
						<view class="cu-tag badge">99</view>
					</view>
					<view class="text-gray">财务账单</view>
				</view>
				<view class="action">
					<view class="cuIcon-cu-image">
						<image src="/static/tabbar/about.png"></image>
						<view class="cu-tag badge"></view>
					</view>
					<view class="text-gray">我的</view>
				</view>
			</view>
			
		</view>
	</view>
	</div>
</template>

<script>
export default {
	data() {
		return {
			cardCur: 0,
			swiperList: [{
				id: 0,
				type: 'image',
				url: '/static/ad1.png',
				// url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big84000.jpg'
			}, {
				id: 1,
				type: 'image',
				url: '/static/ad2.png',
				// url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big37006.jpg',
			}, {
				id: 2,
				type: 'image',
				url: '/static/ad3.png',
				// url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big39000.jpg'
			}, {
				id: 3,
				type: 'image',
				url: '/static/ad4.png',
				// url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big10001.jpg'
			}
			// , {
			// 	id: 4,
			// 	type: 'image',
			// 	url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
			// }, {
			// 	id: 5,
			// 	type: 'image',
			// 	url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big21016.jpg'
			// }, {
			// 	id: 6,
			// 	type: 'image',
			// 	url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big99008.jpg'
			// },
			],
			// 把图片下方小点点变成小长条 true/false
			dotStyle: true,
			
			cuIconList: [{
				cuIcon: 'profile',
				color: 'blue',
				badge: 120,
				name: '门禁进出'
			}, {
				cuIcon: 'moneybag',
				color: 'orange',
				badge: 1,
				name: '账单管理'
			}, {
				cuIcon: 'qr_code',
				color: 'yellow',
				badge: 0,
				name: '访客邀请'
			}, {
				cuIcon: 'repair',
				color: 'olive',
				badge: 22,
				name: '维护报修'
			}, {
				cuIcon: 'location',
				color: 'cyan',
				badge: 0,
				name: '我的车位'
			}, {
				cuIcon: 'service',
				color: 'blue',
				badge: 0,
				name: '我的管家'
			}, {
				cuIcon: 'light',
				color: 'purple',
				badge: 0,
				name: '我的电表'
			}, {
				cuIcon: 'pick',
				color: 'mauve',
				badge: 0,
				name: '送水服务'
			}, {
				cuIcon: 'taxi',
				color: 'purple',
				badge: 0,
				name: '预约洗车'
			}, {
				cuIcon: 'cart',
				color: 'mauve',
				badge: 0,
				name: '商家活动'
			}, {
				cuIcon: 'home',
				color: 'blue',
				badge: 0,
				name: '房屋出租'
			}, {
				cuIcon: 'lock',
				color: 'purple',
				badge: 0,
				name: '智能门锁'
			}, {
				cuIcon: 'safe',
				color: 'mauve',
				badge: 0,
				name: '医疗服务'
			}, {
				cuIcon: 'question',
				color: 'purple',
				badge: 0,
				name: '投诉建议'
			}, {
				cuIcon: 'apps',
				color: 'mauve',
				badge: 0,
				name: '更多功能'
			}],
			modalName: null,
			gridCol: 4,
			gridBorder: false,
			menuBorder: false,
			menuArrow: false,
			menuCard: false,
			skin: false,
			listTouchStart: 0,
			listTouchDirection: null,
		};
	},
	methods: {
		showModal(e) {
			this.modalName = e.currentTarget.dataset.target
		},
		hideModal(e) {
			this.modalName = null
		},
		Gridchange(e) {
			this.gridCol = e.detail.value
		},
		Gridswitch(e) {
			this.gridBorder = e.detail.value
		},
		MenuBorder(e) {
			this.menuBorder = e.detail.value
		},
		MenuArrow(e) {
			this.menuArrow = e.detail.value
		},
		MenuCard(e) {
			this.menuCard = e.detail.value
		},
		SwitchSex(e) {
			this.skin = e.detail.value
		},
		DotStyle(e) {
			this.dotStyle = e.detail.value
		},
		// cardSwiper
		cardSwiper(e) {
			this.cardCur = e.detail.current
		},
	}
};
</script>

<style>
	.box {
		margin: 20upx 0;
	}
	
	.box view.cu-bar {
		height: 100upx;
		width: 100%;
		position: fixed;
		bottom: 0;
	}
	
	.title-bar {
		margin: 100upx 0;
	}
	
	.title-bar view.cu-bar {
		height: 100upx;
		width: 100%;
		position: fixed;
		top: 0;
		z-index: 100;
	}
</style>
