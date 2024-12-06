<template>
  <div class="container mx-auto px-4 py-6">
    <div class="grid gap-6">
      <!-- Room Card -->
      <div
        v-for="deal in deals"
        :key="deal.id"
        class="border rounded-lg shadow-sm overflow-hidden flex"
      >
        <!-- Room Image -->
        <img
          :src="deal.image"
          alt="Room"
          class="w-1/3 object-cover"
        />

        <!-- Room Details -->
        <div class="p-4 flex-1">
          <h3 class="text-lg font-semibold">{{ deal.name }}</h3>
          <p class="text-gray-500 text-sm">{{ deal.description }}</p>
          <p class="text-green-500 text-sm">
            {{ deal.cancellation }}
          </p>
          <p class="mt-2 text-gray-700">
            <span class="text-lg font-semibold">{{ deal.price }}</span>
            / night
          </p>
          <p class="text-gray-500 text-xs">Total: {{ deal.total }}</p>
          <button
            class="px-4 py-2 bg-blue-500 text-white rounded-lg text-sm mt-4"
            @click="bookNow(deal.id)"
          >
            Book Now
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    deals: Array,
  },
  methods: {
    bookNow(id) {
      alert(`Booking room ID: ${id}`);
    },
  },
};
</script>
