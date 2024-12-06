<template>
  <div>
    <SearchBar @search="handleSearch" />
    <Tabs />
  </div>
</template>

<script>
import SearchBar from "../components/SearchBar.vue";

export default {
  components: { SearchBar },
  methods: {
    handleSearch(query) {
      console.log("Search query:", query);
      // Implement API search call here
    },
  },
};
</script>
