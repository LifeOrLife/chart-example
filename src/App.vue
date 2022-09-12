<script lang="ts" setup>
import { ref } from 'vue'
import Earth from './Main.vue'
import LineAni from './LineAni.vue'

const current = ref('')
const goto = (v: string) => {
	current.value = v
}
</script>
<template>
	<div class="con">
		<div class="main" v-if="!current">
			<div class="earth" @click="goto('earth')">地球</div>
			<div class="line" @click="goto('line')">路径动画</div>
		</div>
		<Earth v-if="current === 'earth'" />
		<LineAni v-if="current === 'line'" />
	</div>
</template>
<style scoped>
.con {
	height: 100vh;
	width: 100vw;
	overflow: hidden;
	display: flex;
	align-items: center;
	justify-content: center;
	background: linear-gradient(-45deg, #2edce26e, transparent);
}
.main {
	display: flex;
}
.earth,.line {
	margin: 0 10px;
	color: rgba(102, 204, 255);
	opacity: .6;
	cursor: pointer;
}
.earth:hover,.line:hover {
	opacity: 1;
}
</style>