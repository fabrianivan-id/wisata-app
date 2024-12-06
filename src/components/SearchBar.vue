<template>
	<header class="search-header">
		<div class="search-bar">
			<div class="logo-container">
				<img src="src/assets/logo.png" alt="Wisata App" class="logo" />
			</div>

			<!-- Destination -->
			<div class="destination-container">
				<input
					type="text"
					v-model="form.destination"
					@input="fetchSuggestions"
					placeholder="Where are you going?"
					class="input destination"
					@focus="showMenu('destination')"
				/>
				<ul
					v-if="suggestions.length && activeMenu === 'destination'"
					class="dropdown"
				>
					<li
						v-for="(suggestion, index) in suggestions"
						:key="index"
						@click="selectDestination(suggestion)"
						class="dropdown-item"
					>
						{{ suggestion.name }}
					</li>
				</ul>
			</div>

			<!-- Check-in & Check-out -->
			<div class="date-container">
				<input
					readonly
					:value="dateRangeLabel"
					placeholder="Check-in - Check-out"
					class="input date-range"
					@click="toggleMenu('datepicker')"
				/>
				<div
					v-if="activeMenu === 'datepicker'"
					class="datepicker-popup"
				>
					<Datepicker
						v-model="dateRange"
						range
						@update:modelValue="setDateRange"
						:input-props="{ readonly: true }"
						placeholder="Select Dates"
					/>
				</div>
			</div>

			<!-- Guests and Rooms -->
			<div class="group">
				<div class="dropdown-trigger" @click="toggleMenu('guest')">
					<p class="room-label">{{ roomGuestLabel }}</p>
				</div>
				<div
					v-if="activeMenu === 'guest'"
					class="dropdown guest-dropdown"
				>
					<div class="counter">
						<label>Guests:</label>
						<div class="counter-controls">
							<button
								class="counter-btn"
								@click="decrement('guests_per_room')"
							>
								-
							</button>
							<span class="counter-value">{{
								form.guests_per_room
							}}</span>
							<button
								class="counter-btn"
								@click="increment('guests_per_room')"
							>
								+
							</button>
						</div>
						<!-- Info icon for children -->
						<div class="info-icon-container">
							<svg
								class="info-icon"
								xmlns="http://www.w3.org/2000/svg"
								viewBox="0 0 24 24"
								fill="currentColor"
							>
								<path
									d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm0-14c-1.1 0-2 .9-2 2s.9 2 2 2 2-.9 2-2-.9-2-2-2zm1 10h-2v-2h2v2zm0-4h-2V7h2v5z"
								/>
							</svg>
							What about children?
						</div>
					</div>
					<div class="counter">
						<label>Rooms:</label>
						<div class="counter-controls">
							<button
								class="counter-btn"
								@click="decrement('number_of_rooms')"
							>
								-
							</button>
							<span class="counter-value">{{
								roomWording(form.number_of_rooms)
							}}</span>
							<button
								class="counter-btn"
								@click="increment('number_of_rooms')"
							>
								+
							</button>
						</div>
					</div>
				</div>
			</div>

			<!-- Search Button -->
			<button @click="submitSearch" class="search-btn">Search</button>
		</div>
	</header>
</template>

<script>
	import axios from 'axios'
	import Datepicker from '@vuepic/vue-datepicker'
	import '@vuepic/vue-datepicker/dist/main.css'

	export default {
		components: {
			Datepicker
		},
		data() {
			return {
				form: {
					destination: '',
					checkin: '',
					checkout: '',
					guests_per_room: 1,
					number_of_rooms: 1
				},
				dateRange: [],
				suggestions: [],
				activeMenu: null // Tracks which menu is open (null, 'destination', 'datepicker', 'guest')
			}
		},
		computed: {
			dateRangeLabel() {
				if (this.form.checkin && this.form.checkout) {
					const checkinDate = new Date(this.form.checkin)
					const checkoutDate = new Date(this.form.checkout)

					const checkinDay = checkinDate.getDate()
					const checkoutDay = checkoutDate.getDate()
					const checkinMonth = checkinDate.toLocaleString('en-US', {
						month: 'short'
					})
					const checkoutMonth = checkoutDate.toLocaleString('en-US', {
						month: 'short'
					})

					return checkinMonth === checkoutMonth
						? `${checkinDay}-${checkoutDay} ${checkinMonth}`
						: `${checkinDay} ${checkinMonth} - ${checkoutDay} ${checkoutMonth}`
				}
				return 'Check-in - Check-out'
			},
			roomGuestLabel() {
				const roomCount = this.form.number_of_rooms
				const guestCount = this.form.guests_per_room
				const guestLabel = guestCount === 1 ? 'Guest' : 'Guests'
				return `${this.roomWording(
					roomCount
				)} × ${guestCount} ${guestLabel}`
			}
		},
		methods: {
			async fetchSuggestions() {
				if (this.form.destination.length < 3) {
					this.suggestions = []
					return
				}
				try {
					const response = await axios.get(
						'https://project-technical-test-api.up.railway.app/property/search',
						{
							params: { query: this.form.destination }
						}
					)
					this.suggestions = response.data.map((item) => ({
						id: item.id,
						name: item.name
					}))
				} catch (error) {
					console.error('Error fetching suggestions:', error)
					this.suggestions = []
				}
			},
			selectDestination(suggestion) {
				this.form.destination = suggestion.name
				this.suggestions = []
				this.closeMenus()
			},
			toggleMenu(menu) {
				this.activeMenu = this.activeMenu === menu ? null : menu
			},
			showMenu(menu) {
				this.activeMenu = menu
			},
			closeMenus() {
				this.activeMenu = null
			},
			setDateRange([start, end]) {
				this.form.checkin = start?.toISOString().split('T')[0] || ''
				this.form.checkout = end?.toISOString().split('T')[0] || ''
				this.closeMenus()
			},
			roomWording(roomCount) {
        switch (roomCount) {
          case 1:
            return 'Single Room';
          case 2:
            return 'Double Room';
          case 3:
            return 'Triple Room';
          default:
            return `${roomCount} Rooms`; // New logic
        }
      },
			increment(field) {
				if (field === 'number_of_rooms' && this.form[field] < 10) {
					this.form[field]++
				}
				if (field === 'guests_per_room' && this.form[field] < 10) {
					this.form[field]++
				}
			},
			decrement(field) {
				if (this.form[field] > 1) {
					this.form[field]--
				}
			},
			submitSearch() {
				console.log('Search submitted with data:', this.form)
			}
		},
		mounted() {
			// Close menus if clicked outside
			document.addEventListener('click', (e) => {
				if (!this.$el.contains(e.target)) {
					this.closeMenus()
				}
			})
		}
	}
</script>

<style scoped>
	.search-header {
	  display: flex;
	  position: absolute;
	  top: 0%;
	  left: 0;
	  width: 100%;
	  align-items: center;
	  justify-content: space-between;
	  align-items: center;
	  background-color: #333;

	  color: #fff;
	}

	.logo-container {
	  padding: 10px 20px;
	  flex-shrink: 0;
	}

	.logo {
	  height: 40px;
	}

	.search-bar {
	  display: flex;
	  align-items: center;
	  gap: 15px;
	  flex: 1;
	  background-color: #fff;
	}

	.input {
	  padding: 6px;
	  border-radius: 6px;
	  border: 1px solid #666;
	  color: #333;
	  background-color: #f9f9f9;
	  min-width: 140px;
	}
	.room-label {
	  color: rgba(0, 0, 0, 0.8);
	}
	.guest-dropdown {
	  display: flex;
	  flex-direction: column;
	  gap: 15px;
	  padding: 15px;
	  border: 1px solid #ccc;
	  background-color: #fff;
	  border-radius: 5px;
	  width: 250px;
	  z-index: 100;
	}

	.counter {
	  display: flex;
	  flex-direction: column;
	  gap: 5px;
	}

	.counter label {
	  font-size: 14px;
	  font-weight: bold;
	  color: #333;
	}

	.counter-controls {
	  display: flex;
	  align-items: center;
	  justify-content: space-between;
	  gap: 10px;
	  background-color: #f9f9f9;
	  padding: 8px 10px;
	  border-radius: 5px;
	  border: 1px solid #ddd;
	}

	.counter-btn {
	  background-color: #007bff;
	  color: #fff;
	  font-size: 16px;
	  font-weight: bold;
	  border: none;
	  padding: 5px 10px;
	  border-radius: 5px;
	  cursor: pointer;
	  transition: background-color 0.3s;
	}

	.counter-btn:hover {
	  background-color: #0056b3;
	}

	.counter-value {
	  font-size: 16px;
	  font-weight: bold;
	  color: #333;
	  min-width: 50px;
	  text-align: center;
	}

	.info-icon-container {
	  position: relative;
	  display: inline-block;
	  margin-left: 10px;
	  cursor: pointer;
	  color: rgba(0, 0, 0, 0.8);
	  text-align: center;
	}

	.info-icon {
	  width: 18px;
	  height: 18px;
	  color: #007bff;
	  transition: color 0.3s;
	}

	.info-icon-container:hover .info-icon {
	  color: #0056b3;
	}

	.tooltip {
	  display: none;
	  position: absolute;
	  bottom: 100%;
	  left: 50%;
	  transform: translateX(-50%);
	  background-color: rgba(0, 0, 0, 0.8);
	  color: #fff;
	  padding: 5px 8px;
	  border-radius: 4px;
	  font-size: 12px;
	  white-space: nowrap;
	  z-index: 10;
	}

	.info-icon-container:hover .tooltip {
	  display: block;
	}

	.datepicker-popup,
	.guest-dropdown {
	  position: absolute;
	  background-color: #fff;
	  border: 1px solid #ccc;
	  border-radius: 5px;
	  padding: 10px;
	  z-index: 100;
	}

	.search-btn {
	  background-color: #007bff;
	  color: #fff;
	  padding: 10px 20px;
	  border: none;
	  border-radius: 5px;
	  cursor: pointer;
	}
</style>
