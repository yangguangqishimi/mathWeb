<template>
	<div class="alert alert-secondary">
		<h3>正确: {{stats.success}}</h3>
		<h3>错误: {{stats.error}}</h3>
		<h3>时间: {{stats.time}}分钟</h3>
		<hr>
		<div class="buttons">
			<button class="btn btn-primary" @click="$emit('repeat')">再来一局</button>
			<button class="btn btn-primary" @click="$emit('nextLevel')" v-if="level < 2">下一级</button>
			<button class="btn btn-success" @click="$emit('eraseGame')">新游戏</button>
		</div>				
	</div>
</template>

<script>
export default {
	props:['stats','level']
}
	
</script>

<style scoped>
	
.buttons{
	display: flex;
	justify-content: space-around;
}
</style>