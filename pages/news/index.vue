<template>
	<main class="page page--news">
		<header-image-full :image-bg="featuredImageUrl"/>
		<h2>{{ pageTitle }}</h2>
		<p>{{ featuredImageUrl }}</p>
		<div :v-html="pageContent" class="content"/>
		<home-news-block v-for="(article, index) in homeArticles"
			:key="index"
			:news-item-title="article.title.rendered"
			:news-item-description="article.excerpt.rendered"
			news-item-image="url"/>
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
			pageData = undefined,
			self = this,
			submenu = [],
			articles = [];
		await Promise.all([
			axios.get(apiDomain + '/wp/v2/pages/17').then(async result => {
				pageData = result.data;
			}),
			axios.get(apiDomain + '/wp/v2/posts').then(async result => {
				articles = result.data;
				console.log(articles);
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
			homeArticles: articles
		};
	}
};
</script>

<style lang="scss">
@import '~tools';

.page {
	// general page styles
	&--news {
		// specific home styles
	}
}
</style>
