<template>
  <div class="page-ctn">
    <div class="search-header">
      <div class="input-ctn come-up-sm" v-if="!searchVal">
        <SearchIcon />
        <input
          type="text"
          @input="searchPhotos"
          placeholder="Search for photo"
        />
      </div>
      <div class="search-result come-up-sm" v-if="!loading && searchVal">
        <span>Search Results for</span
        ><span class="query"> "{{ searchVal }}" </span>
        <CloseIcon @click="clearSearch" />
      </div>
    </div>
    <div class="photo-grid come-up-sm" v-if="loading">
      <LoadingCard v-for="i in 10" :key="i" class="card" />
    </div>
    <div
      class="photo-grid come-up-sm"
      v-else-if="!loading && photos.length > 0"
    >
      <PhotoCard
        v-for="photo in photos"
        :key="photo.id"
        :photo="photo"
        class="card"
        @showModal="openModal"
      />
    </div>
    <div class="empty-screen come-up-sm" v-else>
      <Empty />
      <p>No photos found</p>
    </div>

    <PhotoModal
      :photo="selectedPhoto"
      v-if="showPhotoModal"
      @closeModal="showPhotoModal = false"
    />
  </div>
</template>

<script setup>
import axios from "axios";
import { onMounted } from "vue";
import SearchIcon from "@/assets/icons/search.svg?component";
import Empty from "@/assets/icons/empty.svg?component";
import CloseIcon from "@/assets/icons/close.svg?component";

const router = useRouter();
const route = useRoute();
const photos = ref([]);
const searchVal = ref("");
const loading = ref(true);
const showPhotoModal = ref(false);
const selectedPhoto = ref(null);
let timeout = null;

const openModal = (photo) => {
  selectedPhoto.value = photo;
  showPhotoModal.value = true;
};

const getRandomSpan = () => {
  return Math.floor(Math.random() * 3) + 4;
};

const searchPhotos = (e) => {
  clearTimeout(timeout);
  let value = e.target.value;
  loading.value = true;
  timeout = setTimeout(() => {
    listPhotos(value);
  }, 1000);
};

const clearSearch = () => {
  searchVal.value = "";
  router.push({ query: { search: "" } });
  loading.value = true;
  listPhotos("african");
};

const listPhotos = async (search = route.query.search || "african") => {
  let response;
  try {
    response = await axios.get(
      `https://api.unsplash.com/search/photos/?client_id=-CCamJPxZy4dNfXYv4yNNrAg8-Y0i0DsLqH_UtAo9LQ&query=${search}&per_page=8`
    );

    if (response) {
      photos.value = response.data.results;

      if (search != "african") {
        router.push({ query: { search: search } });
        searchVal.value = search;
      }
      loading.value = false;
    }
  } catch (error) {
    console.error(error);
  }
};

onMounted(() => {
  listPhotos();
});
</script>

<style lang="scss">
.search-header {
  width: 100%;
  height: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgb(221, 227, 232);

  .input-ctn {
    width: 100%;
    max-width: 1200px;
    display: flex;
    align-items: center;
    background-color: #fff;
    border-radius: 5px;
    padding: 10px 20px;

    input {
      border: none;
      background-color: transparent;
      outline: none;
      width: 100%;
      padding: 10px;
      padding-left: 20px;

      &::placeholder {
        color: #7d7d7d;
      }
    }

    svg {
      width: 20px;
      color: #7d7d7d;
    }
  }

  .search-result {
    width: 100%;
    max-width: 1068px;

    span {
      font-size: 2rem;
      font-weight: 500;
    }

    .query {
      opacity: 0.7;
      text-transform: capitalize;
    }

    svg {
      position: relative;
      top: -5px;
      width: 24px;
      cursor: pointer;
    }
  }
}

.photo-grid {
  position: relative;
  top: -40px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-auto-rows: 50px;
  row-gap: 30px;
  column-gap: 50px;
  padding: 0 20px;
  max-width: 1024px;
  margin: 0 auto;

  .card {
    &:nth-child(3n) {
      grid-row-end: span 5;
    }
    &:nth-child(3n + 1) {
      grid-row-end: span 4;
    }

    &:nth-child(3n + 2) {
      grid-row-end: span 6;
    }
  }
}

.empty-screen {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 60vh;
  max-width: 1024px;
  margin: 0 auto;

  svg {
    width: 100px;
    height: 100px;
  }

  p {
    font-size: 1.25rem;
    margin-top: 20px;
  }
}
</style>
