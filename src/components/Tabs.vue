<template>
  <div>
    <div class="flex border-b">
      <button
        v-for="tab in tabs"
        :key="tab"
        :class="{
          'border-indigo-500 text-indigo-600': currentTab === tab,
          'border-transparent text-gray-500 hover:text-gray-700': currentTab !== tab,
        }"
        class="flex-1 py-4 px-1 text-center border-b-2 font-medium text-sm"
        @click="currentTab = tab"
      >
        {{ tab }}
      </button>
    </div>
    <div class="p-4">
      <component :is="currentTabComponent" />
    </div>
  </div>
</template>

<script>
import Deals from "./Deals.vue";
import Photos from "./Photos.vue";
import Info from "./Info.vue";

export default {
  components: { Deals, Photos, Info },
  data() {
    return {
      tabs: ["Deals", "Photos", "Info"],
      currentTab: "Deals",
    };
  },
  computed: {
    currentTabComponent() {
      return this.currentTab;
    },
  },
};
</script>
