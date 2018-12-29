<template>
	<article>
		<img :src="newsItemImage">
		<h2>{{ newsItemTitle }}</h2>
		<div v-html="newsItemDescription"></div>
		
	</article>
</template>

<script>
export default {
	props: ['newsItemTitle','newsItemDescription','newsItemImage']
};
</script>

<style lang="scss">
@import '~tools';
// Footer styles
.footer {
	padding: 2rem;
	text-align: center;
}
</style>
