<template>
	<h2>About View</h2>
	<button class="btn btn-primary" @click="goHomePage">Go Home</button>
</template>

<script setup>
import { useRouter } from 'vue-router';

const router = useRouter();

const goHomePage = () => {
	router.push({
		name: 'Home',
	});
};
</script>

<style lang="scss" scoped></style>
