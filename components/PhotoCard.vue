<template>
  <div
    class="card-cover"
    :style="{
      background: `url(${photo.urls.small}) no-repeat center center / cover`,
    }"
    @click="emit('showModal', photo)"
  >
    <div class="img-overlay"></div>
    <div class="photo-info">
      <h3>
        {{ photo.user?.first_name || "-" }}
        {{ photo.user?.last_name || "" }}
      </h3>
      <p>{{ photo.user?.location || "-" }}</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  photo: Object,
});
const emit = defineEmits(["showModal"]);
</script>

<style lang="scss">
.card-cover {
  position: relative;
  overflow: hidden;
  cursor: pointer;

  .img-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 2;
  }

  .photo-info {
    position: relative;
    z-index: 3;
    color: white;
    text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);

    h3 {
      font-size: 1.25rem;
      font-weight: 500;
      margin-bottom: 0.125rem;
    }

    p {
      font-size: 0.875rem;
    }
  }
}
</style>
