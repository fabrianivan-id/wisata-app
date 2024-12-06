<template>
  <div>
    <h2>Info</h2>
    <p>{{ info.description }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      info: {},
    };
  },
  async created() {
    const response = await axios.get(
      "https://project-technical-test-api.up.railway.app/path/to/info"
    );
    this.info = response.data;
  },
};
</script>

<style scoped>
h2 {
  margin-bottom: 10px;
}
p {
  line-height: 1.5;
}
</style>
