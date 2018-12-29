<template>
	<div>
		<h1>{{ pageTitle }}</h1>
		<p>{{ pageContent }}</p>
	</div>
</template>

<script>
const axios = require('axios');
const apiDomain = process.env.apiDomain;

export default {
	async asyncData({ params, error }) {
		let pageData = undefined;
		await Promise.all([
			axios.get(apiDomain + `/wp/v2/posts?slug=${params.id}&_embed`).then(async result => {
				pageData = result.data[0];
				console.log(pageData);
			}),
		]);
		if (pageData === undefined) {
			error({ statusCode: 404, message: 'page not found' });
			return;
		}
		return {
			pageTitle: pageData.title.rendered,
			pageContent: pageData.content.rendered,
			// featuredImageUrl: pageData.better_featured_image.source_url,
		};
	}
};
</script>

<style lang="scss">
@import '~tools';
 
</style>
