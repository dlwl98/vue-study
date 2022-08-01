<template>
	<div>
		<h2>게시글 목록</h2>
		<hr class="my-4" />
		<form @submit.prevent="">
			<div class="row mt-3">
				<div class="col">
					<input v-model="params.title_like" type="text" class="form-control" />
				</div>
				<div class="col-3">
					<select v-model="params._limit" class="form-select">
						<option value="3">3개씩 보기</option>
						<option value="6">6개씩 보기</option>
						<option value="9">9개씩 보기</option>
					</select>
				</div>
			</div>
		</form>
		<hr class="my-4" />
		<div class="row g-3">
			<div v-for="post in posts" :key="post.id" class="col-4">
				<PostItem
					:title="post.title"
					:content="post.content"
					:created-at="post.createdAt"
					@click="goDetailPage(post.id)"
				></PostItem>
			</div>
		</div>
		<AppPagination
			:current-page="params._page"
			:page-count="pageCount"
			@page="page => (params._page = page)"
		/>
	</div>
	<template v-if="posts && posts.length > 0">
		<hr class="my-4" />
		<AppCard>
			<PostDetailView :id="`${posts[0].id}`"></PostDetailView>
		</AppCard>
	</template>
</template>

<script setup>
import PostItem from '@/components/posts/PostItem.vue';
import PostDetailView from './PostDetailView.vue';
import AppCard from '@/components/AppCard.vue';
import { getPosts } from '@/api/posts';
import { ref, watchEffect } from 'vue';
import { useRouter } from 'vue-router';
import { computed } from 'vue';
import AppPagination from '@/components/AppPagination.vue';

const posts = ref([]);
const router = useRouter();
const params = ref({
	_sort: 'createdAt',
	_order: 'desc',
	_page: 1,
	_limit: 9,
	title_like: '',
});
const totalCount = ref(0);
const pageCount = computed(() =>
	Math.ceil(totalCount.value / params.value._limit),
);

const fetchPosts = async () => {
	try {
		const { data, headers } = await getPosts(params.value);
		posts.value = data;
		totalCount.value = headers['x-total-count'];
	} catch (e) {
		console.error(e);
	}
};

const goDetailPage = id => {
	router.push({
		name: 'PostDetail',
		params: {
			id,
		},
	});
};

watchEffect(fetchPosts);
// fetchPosts();
</script>

<style lang="scss" scoped></style>
