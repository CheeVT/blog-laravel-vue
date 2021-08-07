<script>
import { ref, onMounted } from 'vue';

export default {
    setup(props, context) {
        const el = ref(null);

        onMounted(() => {
            resizeTraget(el.value);
        })

        const resizeTraget = (target) => {
            target.style.height = 'auto';
            target.style.height = `${target.scrollHeight}px`;
        }
        
        const resize = (e) => {
            console.log('resized', e);
            resizeTraget(e.target);
        }

        return () => context.slots.default({
            resize,
            el
        });
    }
}
</script>