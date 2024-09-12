<template>
    <div v-if="isVisible" class="modal-backdrop" @click="close">
        <div class="modal-content" @click.stop>
            <header>
                <slot name="header"></slot>
            </header>
            <section>
                <slot name="body"></slot>
            </section>
            <footer>
                <slot name="footer"></slot>
            </footer>
        </div>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
    isOpen: Boolean,
});

const emit = defineEmits(['close']);

const isVisible = ref(props.isOpen)

const close = () => {
    isVisible.value = false;
    emit('close')
};

watch(() => props.isOpen, (newVal) => {
    isVisible.value = newVal
})
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  max-width: 290px;
  max-height: 284px;
}
</style>