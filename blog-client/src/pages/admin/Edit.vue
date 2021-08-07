<template>
    <div>
        <div class="absolute w-full left-0 top-0 p-6 flex justify-between items-center space-x-6">
            <div class="flex-grow flex items-center">
                <span class="mr-1">/</span>
                <input type="text" class="p-0 border-none focus:ring-0 w-full" v-model="post.slug" />
            </div>

            <div class="flex items-center space-x-6">
                <div>
                    <span class="text-sm text-gray-500">Autosaved</span>
                </div>
                <button class="text-sm font-medium">
                    Published
                </button>
                <router-link :to="{ name: 'post', params: { slug: post.slug } }" class="text-sm font-medium text-gray-800">Preview</router-link>
            </div>
        </div>
        <div>
            <ResizeTextarea v-if="post.title">
                <template v-slot:default="{ resize, el }">
                    <textarea :ref="el" v-model="post.title" v-on:input="resize" class="w-full text-center text-4xl lg:text-6xl leading-10 font-bold
                    tracking-tight text-gray-900 border-none focus:ring-0 resize-none p-0"></textarea>
                </template>
            </ResizeTextarea>
        </div>
    </div>
</template>

<script>
import useAdminPosts from '../../api/useAdminPosts';
import { onMounted, watch } from 'vue';
import _ from 'lodash';
import ResizeTextarea from '../../components/ResizeTextarea.vue';

export default {
    components: {
        ResizeTextarea
    },
    props: {
        slug: {
            required: true,
            type: String
        }
    },
    setup(props) {
        const { post, fetchPost, patchPost } = useAdminPosts();

        const updatePost = async () => {
            await patchPost(props.slug);
        }

        onMounted(async () => {
            await fetchPost(props.slug);

            watch(() => _.cloneDeep(post), _.debounce(() => {
                updatePost();
            }, 500));
        });

        return {
            post
        }

    }
}
</script>