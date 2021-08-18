<template>
    <div>
        <div class="absolute w-full left-0 top-0 p-6 flex justify-between items-center space-x-6">
            <div class="flex-grow flex items-center">
                <span class="mr-1">/</span>
                <input type="text" class="p-0 border-none focus:ring-0 w-full" v-model="post.slug" spellcheck="false" v-on:click="$event.target.select()" />
            </div>

            <div class="flex items-center space-x-6">
                <div>
                    <span class="text-sm text-gray-500">{{ lastSaved.fromNow() }}</span>
                </div>
                <button v-on:click="post.published = !post.published" class="text-sm font-medium" v-bind:class="{ 'text-pink-500': !post.published }">
                    {{ !post.published ? 'Unpublished' : 'Published' }}
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

        <Editor v-model:modelValue="post.body" v-model:teaserValue="post.teaser" class="mt-16" />
    </div>
</template>

<script>
import useAdminPosts from '../../api/useAdminPosts';
import { onMounted, watch, watchEffect, ref } from 'vue';
import _ from 'lodash';
import slugify from 'slugify';
import ResizeTextarea from '../../components/ResizeTextarea.vue';
import Editor from '../../components/Editor.vue';
import dayjs from 'dayjs';
import relativeTime from 'dayjs/plugin/relativeTime';
dayjs.extend(relativeTime);

export default {
    components: {
        ResizeTextarea,
        Editor
    },
    props: {
        uuid: {
            required: true,
            type: String
        }
    },
    setup(props) {
        const { post, fetchPost, patchPost } = useAdminPosts();

        const lastSaved = ref(dayjs())

        const updatePost = async () => {
            await patchPost(props.uuid);
            lastSaved.value = dayjs();
        }

        const replaceSlug = () => {
            const slug = post.value.slug;

            if(slug.charAt(slug.length -1) === ' ') {
                return;
            }

            post.value.slug = slug ? slugify(slug, { strict: true }) : post.value.uuid;

        }

        onMounted(async () => {
            await fetchPost(props.uuid);

            watchEffect(() => {
                replaceSlug();
            });

            watch(() => _.cloneDeep(post), _.debounce(() => {
                updatePost();
            }, 500));
        });

        return {
            post,
            lastSaved
        }

    }
}
</script>