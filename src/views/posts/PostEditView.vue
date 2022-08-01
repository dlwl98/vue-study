<template>
	<div>
		<h2>게시글 수정</h2>
		<hr class="my-4" />
	</div>
	<PostForm
		v-model:title="form.title"
		v-model:content="form.content"
		@submit.prevent="edit"
	>
		<template #actions>
			<div class="pt-4">
				<button
					type="button"
					class="btn btn-outline-danger me-2"
					@click="goDetailPage"
				>
					취소
				</button>
				<button class="btn btn-primary">수정</button>
			</div>
		</template>
	</PostForm>
</template>

<script setup>
import { useRoute, useRouter } from 'vue-router';
import { ref } from 'vue';
import { getPostById, updatePost } from '@/api/posts';
import PostForm from '@/components/posts/PostForm.vue';

const route = useRoute();
const router = useRouter();
const id = route.params.id;

const form = ref({
	title: null,
	content: null,
});

const fetchPost = async () => {
	try {
		const { data } = await getPostById(id);
		setForm(data);
	} catch (e) {
		console.error(e);
	}
};

const setForm = ({ title, content }) => {
	form.value.title = title;
	form.value.content = content;
};

fetchPost();

const edit = () => {
	try {
		const data = {
			...form.value,
			createdAt: Date.now(),
		};
		updatePost(id, data);
		router.push({ name: 'PostDetail', id });
	} catch (e) {
		console.error(e);
	}
};

const goDetailPage = () => router.push({ name: 'PostDetail', params: { id } });
</script>

<style lang="scss" scoped></style>
