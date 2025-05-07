<template>
	<div class="side-bar-container">
		<div class="title">
			<el-avatar :src="avatarUrl"></el-avatar>
			<router-link type="primary" class="leave-message" to="/message">
				<i class="el-icon-chat-dot-round"></i>
				博主留言
			</router-link>
			<el-divider><i class="el-icon-user-solid"></i></el-divider>
		</div>
		<p align="center" class="visitor-count">
			Visitor count<br>
			<span class="count">{{ visitorCount }}</span>
		</p>
		<div class="message">
			<ul>
				<li>
					<el-tag type="warning">
						<i class="el-icon-message"></i>
						邮箱: 812853292@qq.com
					</el-tag>
				</li>
				<li>
					<el-tag type="danger">
						<i class="el-icon-phone"></i>
						电话: 13048438815
					</el-tag>
				</li>
			</ul>
		</div>
		<div class="mine">
			<el-card class="box-card">
				<div slot="header" class="clearfix" style="text-align:center;">
					<span>关于我</span>
				</div>
				<div class="text item">
					樱花已灿，雾尽风暖
				</div>
			</el-card>
		</div>
		<el-calendar class="calendar" v-model="value" width="300px" />
	</div>
</template>
  
<script>
export default {
	name: "SideBar",
	data() {
		return {
			value: new Date(),
			visitorCount: 0,
			avatarUrl: require('@/assets/images/avatar.jpg')
		};
	},
	created() {
		this.getVisitorCount();
	},
	methods: {
		async getVisitorCount() {
			try {
				let count = localStorage.getItem('visitorCount');
				if (!count) {
					count = 0;
				}
				count = parseInt(count) + 1;
				localStorage.setItem('visitorCount', count);
				this.visitorCount = count;
			} catch (error) {
				console.error('获取访问计数失败:', error);
			}
		}
	}
};
</script>
  
<style lang="less" scoped>
.side-bar-container {
	display: flex;
	flex-direction: column;
	font-size: 14px;
	padding: 20px;
}

.title {
	text-align: center;
	margin-bottom: 20px;
}

.title .el-avatar {
	width: 120px;
	height: 120px;
	margin-bottom: 15px;
	border: 2px solid #fff;
	box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
	transition: all 0.3s;
}

.title .el-avatar:hover {
	transform: scale(1.05);
	box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.title .leave-message {
	display: block;
	width: fit-content;
	margin: 0 auto;
	font-size: 16px;
	background-image: linear-gradient(to right, orange, purple);
	-webkit-background-clip: text;
	color: transparent;
	text-align: center;
	text-decoration: none;
}

.title .leave-message i {
	font-size: 1.1rem;
}

.visitor-count {
	margin: 20px 0;
	font-size: 16px;
	color: #606266;
}

.visitor-count .count {
	font-size: 24px;
	font-weight: bold;
	color: #409EFF;
}

.message {
	display: block;
	margin: 20px 0;
}

.message ul {
	margin: 0;
	padding: 0;
}

.message ul li {
	width: 100%;
	height: 40px;
	list-style: none;
	margin: 10px 0;
	text-align: center;
}

.message ul li .el-tag {
	width: 100%;
	height: 40px;
	line-height: 40px;
	font-size: 14px;
}

.mine {
	width: 100%;
	margin: 20px 0;
}

.mine .box-card {
	margin-bottom: 1rem;
}

.mine span {
	text-align: center;
	font-weight: 600;
	color: #58666e;
}

.text {
	text-align: center;
	padding: 15px 0;
	font-size: 16px;
	color: #606266;
}

.el-calendar {
	margin-top: 20px;
}

:deep(.el-card) {
	border: 1px solid #EBEEF5;
	background-color: #fafbfc;
	color: #909399;
	transition: .3s;
}

:deep(.el-card__header) {
	padding: 18px 20px;
	border-bottom: 1px solid #e6a23c;
	box-sizing: border-box;
}

:deep(.el-calendar__header) {
	display: flex;
	justify-content: space-between;
	padding: 12px 20px;
	border-bottom: 1px solid #3a8ee6;
}
</style>