<template>
  <div class="home-screen bg-white min-h-screen">
    <!-- Header Section -->
    <header class="text-center pt-16">
      <h1 class="text-4xl font-bold text-gray-800 mb-4">
        The best place to book <span class="text-indigo-600">hotel deals</span>
      </h1>
      <p class="text-gray-600 text-lg">
        Save time and money with our <span class="text-indigo-600 font-semibold">incredible member-only prices</span>
      </p>
      <div class="mt-6 flex justify-center gap-4">
        <button class="btn-primary">Get Started</button>
        <button class="btn-secondary">Download App</button>
      </div>
    </header>

    <!-- Try Section -->
    <div class="text-center mt-8">
      <button class="btn-tertiary">Try it for yourself</button>
    </div>

    <!-- Search Bar -->
    <section class="mt-12">
      <div class="search-bar mx-auto max-w-5xl px-4 py-6 bg-white shadow-lg rounded-lg">
        <div class="flex flex-col md:flex-row gap-4">
          <!-- Destination Field -->
          <div class="flex-1">
            <label for="destination" class="block text-sm font-bold text-gray-600 mb-2">Where are you going?</label>
            <input
              id="destination"
              type="text"
              placeholder="Search for hotels, apartments, or villas"
              class="w-full border border-gray-300 rounded-lg p-3"
              v-model="destination"
            />
          </div>

          <!-- Check In - Check Out -->
          <div class="flex-1">
            <label for="dates" class="block text-sm font-bold text-gray-600 mb-2">Check In - Check Out</label>
            <vue3-date-picker
              v-model="dateRange"
              range
              format="DD MMM YYYY"
              placeholder="Select dates"
              class="w-full"
            />
          </div>

          <!-- Guests and Rooms -->
          <div class="flex-1">
            <label for="guests" class="block text-sm font-bold text-gray-600 mb-2">Guests & Rooms</label>
            <select
              id="guests"
              v-model="guestsAndRooms"
              class="w-full border border-gray-300 rounded-lg p-3"
            >
              <option value="1">1 Room, 1 Guest</option>
              <option value="2">1 Room, 2 Guests</option>
              <option value="3">2 Rooms, 2 Guests</option>
            </select>
          </div>

          <!-- Search Button -->
          <div class="flex items-end">
            <button
              class="bg-indigo-600 text-white px-6 py-3 rounded-lg hover:bg-indigo-700 transition"
              @click="handleSearch"
            >
              Search
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="text-center mt-12">
      <h2 class="text-lg font-bold text-gray-600">Huge selections of hotels</h2>
      <p class="text-sm text-gray-500">We partner with top global hotel chains to bring you the best deals</p>
      <div class="flex justify-center gap-4 mt-4">
        <img src="/path/to/marriott-logo.png" alt="Marriott" class="w-20" />
        <img src="/path/to/hilton-logo.png" alt="Hilton" class="w-20" />
        <img src="/path/to/hyatt-logo.png" alt="Hyatt" class="w-20" />
        <img src="/path/to/ihg-logo.png" alt="IHG" class="w-20" />
      </div>
    </footer>
  </div>
</template>

<script>
import { ref } from "vue";
import "vue3-date-picker/dist/main.css";
import DatePicker from "vue3-date-picker";

export default {
  components: {
    Vue3DatePicker: DatePicker,
  },
  setup() {
    const destination = ref("");
    const dateRange = ref(null);
    const guestsAndRooms = ref("1");

    const handleSearch = () => {
      if (!destination.value || !dateRange.value) {
        alert("Please fill all fields!");
        return;
      }

      const [checkIn, checkOut] = dateRange.value;

      alert(
        `Searching for ${destination.value} from ${checkIn} to ${checkOut} for ${guestsAndRooms.value}`
      );
    };

    return {
      destination,
      dateRange,
      guestsAndRooms,
      handleSearch,
    };
  },
};
</script>

<style scoped>
/* Global Styling */
.btn-primary {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  font-weight: bold;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-primary:hover {
  background-color: #0056b3;
}

.btn-secondary {
  background-color: #f8f9fa;
  color: #007bff;
  padding: 10px 20px;
  border-radius: 5px;
  font-weight: bold;
  border: 1px solid #007bff;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.btn-secondary:hover {
  background-color: #007bff;
  color: white;
}

.btn-tertiary {
  background-color: transparent;
  color: #007bff;
  padding: 10px 20px;
  border: none;
  font-weight: bold;
  text-decoration: underline;
  cursor: pointer;
}

/* Responsive Search Bar */
.search-bar {
  max-width: 100%;
  margin: auto;
}
</style>
