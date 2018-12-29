<template>
	<main class="page page--home">
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
			axios.get(apiDomain + '/wp/v2/pages/12').then(async result => {
				pageData = result.data;
			}),
			axios.get(apiDomain + '/wp/v2/posts').then(async result => {
				articles = result.data;
				console.log(articles);
				// articles.forEach(element => {
				// 	console.log(element);
				// });
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
			// image: featuredImage,
			// date: page.date,
			// title: page.title.rendered,
			// content: page.content.rendered,
			// categories: categories,
			// seo: page.seo
		};
	}
};
</script>

<style lang="scss">
@import '~tools';

.page {
	// general page styles
	&--home {
		// specific home styles
	}
}
</style>
