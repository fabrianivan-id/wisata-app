<template>
  <div>
    <h2>Photos</h2>
    <div class="photos">
      <img 
        v-for="photo in photos" 
        :key="photo.id" 
        :src="photo.url" 
        :alt="photo.caption" 
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      photos: [],
    };
  },
  async created() {
    const response = await axios.get(
      "https://project-technical-test-api.up.railway.app/path/to/photos"
    );
    this.photos = response.data;
  },
};
</script>

<style scoped>
.photos {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
img {
  width: 150px;
  height: 100px;
  object-fit: cover;
}
</style>
