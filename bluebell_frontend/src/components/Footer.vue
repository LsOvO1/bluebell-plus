<template>
	<div>
		<el-row>
			<el-col :span="24">
				<div class="grid-content bg-purple-dark footer">
					<h2 class="compony">风铃草论坛</h2>
				</div>
			</el-col>
		</el-row>
	</div>
</template>

<script>
export default {
	name: "Footer",
};
</script>

<style>
* {
	list-style: none;
	text-decoration: none;
}

.footer {
	min-height: 100px;
	margin-top: 20px;
	border-top: 5px solid #74dfff;
	background-color: rgba(43, 42, 42, 1);
	display: flex;
	justify-content: center;
	align-items: center;
}

.footer .compony {
	color: #fff;
	text-align: center;
	font-size: 18px;
	font-weight: 500;
	letter-spacing: 2px;
}
</style>