<template>
    <EditorContent :editor="editor" />
</template>

<script>
import { useEditor, EditorContent } from '@tiptap/vue-3';
import StarterKit from '@tiptap/starter-kit';

export default {
    components: {
        EditorContent
    },
    setup(props) {
        const editor = useEditor({
            content: 'Hi there',
            extensions: [
                StarterKit
            ],
            editorProps: {
                attributes: {
                    class: 'bg-gray-300'
                }
            }
        })

        return {
            editor
        }
    }
}
</script>