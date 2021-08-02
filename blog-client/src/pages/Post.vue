<template>
    <div>
        <h1 class="block text-center text-4xl sm:text-6xl leading-10 font-bold">
            {{ post.title }}
        </h1>

        <div class="mt-16 text-gray-500 prose prose-sm lg:prose" v-html="post.body"></div>
    </div>
</template>

<script>
import { onMounted } from 'vue';
import usePosts from '../api/usePosts';

export default {
    props: {
        slug: {
            required: true,
            type: String
        }
    },
    setup(props) {
        const { post, fetchPost } = usePosts();

        onMounted(fetchPost(props.slug));

        return {
            post
        }
    }
}
</script>