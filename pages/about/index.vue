<template>
	<main class="page page--about">
		<header-image-full :image-bg="featuredImageUrl"/>
		<h2>{{ pageTitle }}</h2>
		<p>{{ featuredImageUrl }}</p>
		<div :v-html="pageContent" class="content"/>
	</main>
</template>

<script>
const axios = require('axios');
const apiDomain = process.env.apiDomain;

import HeaderImageFull from '~/components/sections/header-image-full.vue';
import HomeNewsBlock from '~/components/blocks/home-news-block.vue';
export default {
	components: {
		HeaderImageFull,
		HomeNewsBlock
	},
	async asyncData({ params, error }) {
		let featuredImage = '/img/placeholder.png',
			pageData = undefined;
			
		await Promise.all([
			axios.get(apiDomain + '/wp/v2/pages/8').then(async result => {
				pageData = result.data;
			})
		]);
		if (pageData === undefined) {
			error({ statusCode: 404, message: 'page not found' });
			return;
		}
		return {
			pageTitle: pageData.title.rendered,
			pageContent: pageData.content.rendered,
			featuredImageUrl: pageData.better_featured_image.source_url,
		};
	}
};
</script>

<style lang="scss">
@import '~tools';

.page {
	// general page styles
	&--about {
		// specific home styles
	}
}
</style>
