<template>
	<div class="row center small-full medium-full large-20">
		<section :style="`background-image: url('${ imageBg }')`" class="section column header-image-full"/>
	</div>
</template>

<script>
export default {
	props: ['imageBg']
};
</script>

<style lang="scss">
@import '~tools';

.header-image-full {
	// Header styles
	background-color: black;
	background-position: center center;
	background-size: cover;
	height: 300px;
}
</style>