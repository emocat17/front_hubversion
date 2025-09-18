<template>
	<view class="driver-dashboard-container p-4 md:p-6">
		<h2 class="text-2xl font-bold text-gray-800 mb-6">下午好, {{ username }}</h2>

		<view class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
			<view v-for="(stat, index) in stats" :key="index" class="bg-white p-6 rounded-lg shadow-md flex items-center justify-between">
				<div>
					<p class="text-gray-500 text-sm">{{ stat.title }}</p>
					<p class="text-3xl font-bold text-gray-800">{{ stat.value }}</p>
				</div>
				<view class="text-right">
					<p :class="stat.changeType === 'positive' ? 'text-green-500' : 'text-red-500'" class="text-sm font-semibold">
						{{ stat.change }}
					</p>
					<p class="text-xs text-gray-400">较昨日</p>
				</view>
			</view>
		</view>

		<view class="grid grid-cols-1 lg:grid-cols-3 gap-6">
			<view class="lg:col-span-1 bg-white p-6 rounded-lg shadow-md">
				<h3 class="text-lg font-bold text-gray-800 mb-4">当前任务</h3>
				<view v-if="currentTask">
					<view class="border rounded-lg p-4 space-y-3">
						<view class="flex justify-between items-center">
							<span class="font-bold text-gray-700">订单号: {{ currentTask.orderId }}</span>
							<span class="px-2 py-1 text-xs rounded-full" :class="currentTask.statusClass">{{ currentTask.statusText }}</span>
						</view>
						<view>
							<p class="text-sm text-gray-500">起点: {{ currentTask.startLocation }}</p>
							<p class="text-sm text-gray-500">终点: {{ currentTask.endLocation }}</p>
						</view>
						<view>
							<p class="text-sm text-gray-500">货物信息: {{ currentTask.cargoInfo }}</p>
							<p class="text-sm text-gray-500">预计送达: {{ currentTask.estimatedDelivery }}</p>
						</view>
						<view class="w-full h-40 bg-gray-200 rounded-md mt-2 flex items-center justify-center">
							<p class="text-gray-500">地图预览</p>
						</view>
						<button class="w-full mt-4 bg-blue-500 text-white py-2 rounded-lg hover:bg-blue-600 transition">
							更新状态
						</button>
					</view>
				</view>
				<view v-else>
					<p class="text-gray-500">暂无进行中的任务。</p>
				</view>
			</view>

			<view class="lg:col-span-2 bg-white p-6 rounded-lg shadow-md">
				<h3 class="text-lg font-bold text-gray-800 mb-4">近期订单列表</h3>
				<view class="overflow-x-auto">
					<table class="w-full text-left text-sm">
						<thead>
							<tr class="text-gray-500 border-b">
								<th class="py-2 px-3 font-medium">订单号</th>
								<th class="py-2 px-3 font-medium">路线</th>
								<th class="py-2 px-3 font-medium">货物信息</th>
								<th class="py-2 px-3 font-medium">状态</th>
								<th class="py-2 px-3 font-medium text-center">操作</th>
							</tr>
						</thead>
						<tbody>
							<tr v-for="order in recentOrders" :key="order.id" class="border-b hover:bg-gray-50">
								<td class="py-3 px-3">{{ order.id }}</td>
								<td class="py-3 px-3">{{ order.route }}</td>
								<td class="py-3 px-3">{{ order.cargoInfo }}</td>
								<td class="py-3 px-3">
									<span class="px-2 py-1 text-xs rounded-full" :class="order.statusClass">{{ order.statusText }}</span>
								</td>
								<td class="py-3 px-3 text-center">
									<button @click="viewOrderDetails(order.id)" class="text-blue-500 hover:underline text-xs">
										查看详情
									</button>
								</td>
							</tr>
						</tbody>
					</table>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	name: "DriverDashboard",
	data() {
		return {
			username: "司机师傅", // 可从用户登录信息中获取
			stats: [{
				title: "待接单",
				value: "12",
				change: "+2%",
				changeType: "positive"
			}, {
				title: "进行中",
				value: "3",
				change: "-1.5%",
				changeType: "negative"
			}, {
				title: "已完成 (本月)",
				value: "58",
				change: "+10%",
				changeType: "positive"
			}, {
				title: "总收入 (本月)",
				value: "¥25,800",
				change: "+8.8%",
				changeType: "positive"
			}],
			currentTask: {
				orderId: "DD20250918-001",
				statusText: "运输中",
				statusClass: "bg-blue-100 text-blue-700",
				startLocation: "上海浦东新区",
				endLocation: "江苏苏州工业园区",
				cargoInfo: "大型机械设备 (25吨)",
				estimatedDelivery: "2025-09-19 14:00"
			},
			recentOrders: [{
				id: "DD20250917-005",
				route: "上海 -> 杭州",
				cargoInfo: "精密仪器 (5吨)",
				statusText: "已完成",
				statusClass: "bg-green-100 text-green-700"
			}, {
				id: "DD20250917-002",
				route: "上海 -> 南京",
				cargoInfo: "建筑材料 (30吨)",
				statusText: "已完成",
				statusClass: "bg-green-100 text-green-700"
			}, {
				id: "DD20250916-011",
				route: "宁波 -> 上海",
				cargoInfo: "风力发电机叶片",
				statusText: "已取消",
				statusClass: "bg-gray-100 text-gray-700"
			}, {
				id: "DD20250915-009",
				route: "上海 -> 无锡",
				cargoInfo: "大型钢材 (28吨)",
				statusText: "已完成",
				statusClass: "bg-green-100 text-green-700"
			}]
		};
	},
	methods: {
		/**
		 * 查看订单详情
		 * @param {string} orderId - 订单ID
		 */
		viewOrderDetails(orderId) {
			console.log("跳转到订单详情页，订单ID:", orderId);
			// 在uni-admin中，您可以使用 uni.navigateTo 进行页面跳转
			// uni.navigateTo({
			//  url: `/pages/orders/detail?id=${orderId}`
			// });
			uni.showToast({
				title: `查看订单: ${orderId}`,
				icon: 'none'
			});
		}
	}
};
</script>

<style scoped>
/* * 该组件的样式完全由 Tailwind CSS 类提供。
 * 请确保您的项目中已安装并配置了 Tailwind CSS。
 * scoped 属性确保这些样式不会影响到组件外部。
*/
.driver-dashboard-container {
	background-color: #f8f9fa;
	min-height: 100vh;
}
</style>