<template>
	<div class="word-of-day" @click="getWordOfDay" :title="wordOfDay">{{ wordOfDay }}</div>
</template>

<script>
export default {
	name: "DayOfWord",
	data() {
		return {
			wordOfDay: '',
			poems: [
				"人生若只如初见，何事秋风悲画扇",
				"曾经沧海难为水，除却巫山不是云",
				"众里寻他千百度，蓦然回首，那人却在灯火阑珊处",
				"衣带渐宽终不悔，为伊消得人憔悴",
				"两情若是久长时，又岂在朝朝暮暮",
				"人生自是有情痴，此恨不关风与月",
				"此情可待成追忆，只是当时已惘然",
				"身无彩凤双飞翼，心有灵犀一点通",
				"问世间情为何物，直教生死相许",
				"十年生死两茫茫，不思量，自难忘"
			]
		};
	},
	created() {
		this.getWordOfDay();
	},
	methods: {
		getWordOfDay() {
			const randomIndex = Math.floor(Math.random() * this.poems.length);
			this.wordOfDay = this.poems[randomIndex];
		},
	}
}
</script>

<style lang="less" scoped>
.word-of-day {
	width: 100%;
	line-height: 32px;
	font-size: 14px;
	background-image: linear-gradient(to right, orange, purple);
	-webkit-background-clip: text;
	color: transparent;
	text-align: center;
	cursor: pointer;
	margin-left: 1rem;
}
</style>