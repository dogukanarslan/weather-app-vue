<template>
  <Teleport to="body">
    <Transition name="modal-outer">
      <div
        v-if="modalActive"
        class="absolute left-0 top-0 flex h-screen w-full justify-center bg-black bg-opacity-30 px-8"
      >
        <Transition name="modal-inner">
          <div class="mt-32 max-w-screen-md self-start bg-white p-4">
            <slot></slot>
            <button
              @click="$emit('close-modal')"
              class="text-shite mt-8 bg-weather-primary px-6 py-2"
            >
              Close
            </button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
defineProps({
  modalActive: { type: Boolean, default: false }
})
defineEmits(["close-modal"])
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-outer-enter-from,
.modal-outer-leave-to {
  opacity: 0;
}
</style>
