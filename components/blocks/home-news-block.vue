<template>
	<a :href="`/news/${newsItemSlug}`">
		<article>
			<img :src="newsItemImage">
			<h2>{{ newsItemTitle }}</h2>
			<div v-html="newsItemDescription"></div>
		</article>
	</a>
</template>

<script>
export default {
	props: ['newsItemTitle','newsItemDescription','newsItemImage','newsItemSlug']
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
