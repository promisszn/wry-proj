<template>
  <div>
    <div class="modal-overlay">
      <div class="modal-content">
        <div class="close-icon" @click="close">
          <CloseIcon />
        </div>

        <div class="content come-up-sm">
          <div
            class="image-bg"
            :style="{
              background: `url(${photo.urls.regular}) no-repeat center center / cover`,
            }"
          ></div>
          <div class="photo-info">
            <h3>
              {{ photo.user?.first_name || "-" }}
              {{ photo.user?.last_name || "" }}
            </h3>
            <p>{{ photo.user?.location || "-" }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted } from "vue";
import CloseIcon from "@/assets/icons/close.svg?component";

const props = defineProps({
  photo: Object,
});

onMounted(() => {
  document.body.style.overflow = "hidden";
});

const emit = defineEmits(["closeModal"]);
const close = () => {
  emit("closeModal");
  document.body.style.overflowY = "scroll";
};
</script>

<style scoped lang="scss">
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.close-icon {
  position: absolute;
  right: 40px;
  top: 40px;
  z-index: 999;

  svg {
    width: 30px;
    height: 30px;
    fill: #f1f1f1;
  }
}

.close-icon:hover {
  scale: 1.1;
  transition: all 0.3s ease;
}

.content {
  width: 100%;
  max-width: 1000px;
  height: 85vh;
  background-color: #fff;
  border-radius: 10px;

  .image-bg {
    width: 100%;
    height: 80%;
    border-radius: 10px 10px 0 0;
    background-color: #f1f1f1;
  }

  .photo-info {
    height: 20%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding-inline: 30px;

    h3 {
      font-size: 1.25rem;
      font-weight: 500;
      margin-bottom: 0.35rem;
    }

    p {
      font-size: 0.875rem;
      opacity: 0.7;
    }
  }
}
</style>
