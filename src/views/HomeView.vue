<template>
	<h2>Home View</h2>
	<button class="btn btn-primary" @click="goAboutPage">Go About</button>
</template>

<script setup>
import { useRouter } from 'vue-router';

const router = useRouter();

const goAboutPage = () => {
	router.push({
		name: 'About',
	});
};
</script>

<style lang="scss" scoped></style>
