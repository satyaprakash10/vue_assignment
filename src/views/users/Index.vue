<template>
  <div class="h-full px-4 py-2 sm:px-0 sm:py-0">
    <!-- User breadcrumb Section -->
    <div
      class="px-4 py-3 mt-10 bg-white border-t-4 border-green-500 rounded-md shadow-lg"
    >
      <nav class="" aria-label="Breadcrumb">
        <div class="flex items-center justify-between">
          <div>
            <h1 class="font-sans text-xl font-bold sm:text-2xl">Users List</h1>
            <p class="mt-2 text-sm text-gray-700">
              A list of all the users in your account including their name,
              title, email and phone.
            </p>
          </div>
          <div class="mt-4 lg:mt-0">
            <router-link
              to="/create"
              class="flex items-center justify-center px-4 py-3 pl-2 font-semibold text-center text-white transition-all duration-300 bg-green-500 border rounded-md hover:ease-in-out hover:bg-green-400 hover:bg-opacity-90 hover:shadow-lg"
            >
              <PlusIcon class="w-6 h-6 mr-2" />
              Add New User
            </router-link>
          </div>
        </div>

        <nav class="flex mt-6 mb-2" aria-label="Breadcrumb">
          <ol role="list" class="flex items-center space-x-4">
            <li>
              <div>
                <a href="#" class="text-gray-400 hover:text-gray-500">
                  <HomeIcon class="flex-shrink-0 w-5 h-5" aria-hidden="true" />
                  <span class="sr-only">Home</span>
                </a>
              </div>
            </li>

            <li v-for="page in pages" :key="page.name">
              <div class="flex items-center">
                <ChevronRightIcon
                  class="flex-shrink-0 w-5 h-5 text-gray-400"
                  aria-hidden="true"
                />
                <a
                  :href="page.href"
                  class="ml-4 text-sm font-medium text-gray-500 hover:text-gray-700"
                  :aria-current="page.current ? 'page' : undefined"
                >
                  {{ page.name }}
                </a>
              </div>
            </li>
          </ol>
        </nav>
      </nav>
    </div>

    <hr class="w-full mx-auto mt-10 border-green-300 max-w-7xl" />

    <!-- Filters Widget -->

    <h1 class="px-2 py-3 mt-4 text-lg font-bold sm:text-2xl">Filter User</h1>

    <div class="flex items-center justify-between">
      <div class="flex items-start space-x-4">
        <input
          type="text"
          v-model="name"
          placeholder="Search Name"
          class="w-full px-4 py-3 transition-all duration-500 ease-in-out transform bg-white border border-green-200 shadow-sm hover:shadow-lg sm:w-60 rounded-2xl hover:bg-green-50 hover:border-green-500 hover:border"
          name=""
          id=""
        />
        <input
          type="text"
          v-model="email"
          placeholder="Search E-mail"
          class="w-full px-4 py-3 transition-all duration-500 ease-in-out transform bg-white border border-green-200 shadow-sm hover:shadow-lg sm:w-60 rounded-2xl hover:bg-green-50 hover:border-green-500 hover:border"
          name=""
          id=""
        />
        <input
          type="text"
          v-model="mobile"
          placeholder="Search Phone"
          class="w-full px-4 py-3 transition-all duration-500 ease-in-out transform bg-white border border-green-200 shadow-sm hover:shadow-lg sm:w-60 rounded-2xl hover:bg-green-50 hover:border-green-500 hover:border"
          name=""
          id=""
        />
      </div>

      <div class="flex items-center justify-end space-x-4">
        <ViewGridIcon
          @click="showUserList"
          class="w-12 h-12 p-2 text-white transition-all duration-500 ease-in bg-green-500 rounded-md shadow-md cursor-pointer hover:shadow-lg hover:bg-green-300"
        />
        <MapIcon
          @click="showMapView"
          class="w-12 h-12 p-2 text-white transition-all duration-500 ease-in bg-green-500 rounded-md shadow-md cursor-pointer hover:shadow-lg hover:bg-green-300"
        />
      </div>
    </div>

    <hr class="w-full mx-auto mt-10 border-green-300 max-w-7xl" />

    <!-- User Table View -->
    <transition name="fade">
      <div class="w-full px-5 py-2 mt-8" v-show="showUserListView">
        <div class="">
          <div class="overflow-auto lg:overflow-visible" v-if="getAllUsers">
            <table
              style="width: 100%"
              class="table space-y-6 text-sm text-gray-600 border-separate"
            >
              <thead class="w-full text-white bg-gray-800" style="width: 100%">
                <tr class="uppercase">
                  <th class="p-3 text-left">ID</th>
                  <th class="p-3 text-left">Image</th>
                  <th class="p-3 text-left">First Name</th>
                  <th class="p-3 text-left">Mobile</th>
                  <th class="p-3 text-left">Email</th>
                  <th class="p-3 text-center">Action</th>
                </tr>
              </thead>

              <tbody>
                <tr
                  class="py-3 transition-all duration-500 bg-white border shadow cursor-pointer hover:border-b-4 group hover:border-green-500 hover:ease-out hover:bg-gray-200 hover:text-green-500 hover:shadow-xl"
                  v-for="(user, userIdx) in filterUsers"
                  :key="userIdx"
                >
                  <td class="px-4">
                    <div
                      class="w-10 h-10 p-2 font-bold text-center bg-green-100 shadow-md group-hover:text-black group-hover:bg-gray-600 group-hover:bg-opacity-40"
                    >
                      {{ user.id }}
                    </div>
                  </td>
                  <td
                    v-if="user.image"
                    class=""
                    @click="$router.push(`/${user.id}/edit`)"
                  >
                    <img
                      :src="user.image"
                      class="w-20 h-20 rounded-full shadow-md"
                      alt="user_image"
                    />
                  </td>

                  <td class="" v-else>
                    <img
                      src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
                      class="w-20 h-20 rounded-full shadow-md"
                      alt="user_image"
                    />
                  </td>
                  <td
                    class="p-3 text-base font-semibold text-left text-indigo-500 hover:text-black"
                    @click="$router.push(`/${user.id}/edit`)"
                  >
                    {{ user.name }}
                  </td>

                  <td class="max-w-sm p-3 font-bold truncate">
                    + {{ user.mobile }}
                  </td>

                  <td class="max-w-sm p-3 font-bold truncate">
                    {{ user.email }}
                  </td>

                  <div
                    class="flex items-center justify-center mt-4 space-x-4 text-center hover:cursor-pointer"
                  >
                    <td
                      @click="$router.push(`/${user.id}`)"
                      class="w-10 h-10 px-2.5 transition-all duration-700 ease-in-out text-sm font-medium text-left text-gray-900 cursor-pointer hover:bg-gray-900 hover:bg-opacity-40 rounded-full hover:rounded-full hover:text-white hover:shadow-xl whitespace-nowrap"
                    >
                      <EyeIcon class="w-5 h-5 mt-2" />
                    </td>
                    <td
                      @click="alertUser(user.id)"
                      class="w-10 h-10 px-2.5 transition-all duration-700 ease-in-out text-sm font-medium text-left text-gray-900 cursor-pointer hover:bg-gray-900 hover:bg-opacity-40 rounded-full hover:rounded-full hover:text-red-500 hover:shadow-xl whitespace-nowrap"
                    >
                      <TrashIcon class="w-5 h-5 mt-2" />
                    </td>
                    <td
                      class="w-10 h-10 px-2.5 transition-all duration-700 ease-in-out text-sm font-medium text-left text-gray-900 cursor-pointer hover:bg-gray-900 hover:bg-opacity-40 rounded-full hover:rounded-full hover:text-white hover:shadow-xl whitespace-nowrap"
                    >
                      <router-link :to="`/${user.id}/edit`">
                        <PencilIcon class="w-5 h-5 mt-2" />
                      </router-link>
                    </td>
                  </div>
                </tr>
              </tbody>
            </table>
          </div>

          <div v-else>
            <div
              class="px-6 py-6 mt-4 text-lg font-bold text-center text-gray-500 bg-gray-100 rounded-md shadow-md"
            >
              <h3>No Users List found yet!</h3>
              <div class="flex justify-center">
                <router-link
                  to="/user-create"
                  class="flex px-4 py-3 mt-4 transition-all duration-300 border border-indigo-500 rounded-md w-52 hover:ease-in-out hover:bg-indigo-500 hover:bg-opacity-30 hover:shadow-lg"
                >
                  <PlusIcon class="w-6 h-6 ml-2 mr-3" />

                  Add New User
                </router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <div class="px-4 mt-5">
      <h1 class="font-serif text-lg font-extrabold text-gray-600 sm:text-3xl">
        See Users on Map View
      </h1>
    </div>

    <div class="px-4 mx-auto mt-10">
      <div
        id="map"
        style="
          width: 100% !important;
          height: 500px !important;
          border-radius: 14px;
        "
      ></div>
    </div>

    <!-- Users on Map View -->
    <transition name="fade">
      <div class="px-4 py-2 mt-10" v-show="showUserMapView">
        <div
          id="map"
          style="
            width: 100% !important;
            height: 500px !important;
            border-radius: 14px;
          "
        ></div>
      </div>
    </transition>
  </div>
</template>

<script>
import {
  HomeIcon,
  PlusIcon,
  TrashIcon,
  PencilIcon,
  EyeIcon,
  ChevronRightIcon,
  ViewGridIcon,
  MapIcon,
} from '@vue-hero-icons/solid'
import { mapActions, mapGetters } from 'vuex'
import axios from 'axios'
export default {
  components: {
    HomeIcon,
    PlusIcon,
    TrashIcon,
    PencilIcon,
    EyeIcon,
    ChevronRightIcon,
    ViewGridIcon,
    MapIcon,
  },

  data: () => ({
    name: '',
    email: '',
    mobile: '',

    pages: [
      { name: 'Home', href: '/', current: false },
      { name: 'Users', href: '#', current: true },
    ],

    showUserListView: true,
    showUserMapView: false,
  }),

  computed: {
    ...mapGetters(['getAllUsers']),

    filterUsers: function () {
      return this.getAllUsers
        .filter(this.filterByName)
        .filter(this.filterByEmail)
        .filter(this.filterByMobile)
    },
  },

  mounted() {
    this.fetchAllUsers()
    if (this.getAllUsers) {
      axios.get('http://localhost:3000/users').then((res) => {
        if (res) {
          this.initMap(res.data)
        }
      })
    }
  },

  methods: {
    ...mapActions(['fetchAllUsers', 'deleteUser']),

    // Filters methods
    filterByName: function (user) {
      if (this.name.length === 0) {
        return true
      }

      return user.name.toLowerCase().indexOf(this.name.toLowerCase()) > -1
    },

    filterByEmail: function (user) {
      if (this.email.length === 0) {
        return true
      }

      return user.email.toLowerCase().indexOf(this.email.toLowerCase()) > -1
    },

    filterByMobile: function (user) {
      if (this.mobile.length === 0) {
        return true
      }

      return user.mobile.toLowerCase().indexOf(this.mobile.toLowerCase()) > -1
    },

    // Show user list view
    showUserList() {
      this.showUserListView = true

      this.showUserMapView = false
    },

    showMapView() {
      this.showUserListView = false

      this.showUserMapView = true
    },

    initMap(users) {
      mapboxgl.accessToken =
        'pk.eyJ1IjoiaGFzaWkiLCJhIjoiY2wwYWt4cjJ1MDF4YTNjbWptNzZwM2ZhZyJ9.9WUVDkgiL5lsqX95u0VAvA'

      const map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v11',
        center: [-120.094292506456, 48.0250054263],
        zoom: 10,
      })

      var features = []

      for (let index = 0; index < users.length; index++) {
        const element = users[index]
        features.push({
          type: 'Feature',
          properties: {
            description: `
             <div class="flex justify-between space-x-4">
               <img src="${element.image}" class="w-10 h-10" />
               <div>
                 <strong> ${element.name}  </strong>
                 <p> ${element.address} </p>
               </div>
             </div>
           `,
            property: element,
          },
          geometry: {
            type: 'Point',
            coordinates: element.coordinates,
          },
        })
      }

      map.on('load', () => {
        map.addSource('places', {
          type: 'geojson',
          data: {
            type: 'FeatureCollection',
            features: features,
          },
        })

        // Add a marker on map to show the user details

        map.addLayer({
          id: 'places',
          type: 'circle',
          source: 'places',
          paint: {
            'circle-color': '#4264fb',
            'circle-radius': 8,
            'circle-stroke-width': 2,
            'circle-stroke-color': '#ffffff',
          },
        })

        const popup = new mapboxgl.Popup({
          closeButton: false,
          closeOnClick: false,
        })

        map.on('mouseenter', 'places', (e) => {
          map.getCanvas().style.cursor = 'pointer'

          // Copy coordinates array.
          const coordinates = e.features[0].geometry.coordinates.slice()
          const description = e.features[0].properties.description

          while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
            coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360
          }

          popup.setLngLat(coordinates).setHTML(description).addTo(map)
        })

        map.on('mouseleave', 'places', () => {
          map.getCanvas().style.cursor = ''
          popup.remove()
        })

        map.on('click', 'places', (e) => {
          this.show = true
          this.selectedProperty = JSON.parse(e.features[0].properties.property)
        })

        map.addSource('states', {
          type: 'geojson',
          data: 'https://docs.mapbox.com/mapbox-gl-js/assets/us_states.geojson',
        })
      })
    },

    // Delete User Alert
    alertUser(id) {
      this.$swal
        .fire({
          title: 'Are you sure?',
          text: 'You want to delete this user!',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes',
        })
        .then((result) => {
          if (result.isConfirmed) {
            this.removeUser(id)
          }
        })
    },

    removeUser(id) {
      this.deleteUser(id)
        .then(() => {
          this.$toastr.s('User deleted successfully')
        })
        .catch((err) => {
          this.$toastr.e(err.response.data)
        })
    },
  },
}
</script>

<style scoped lang="css">
::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 10px;
  overflow: hidden;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}

.table {
  border-spacing: 0 18px;
  width: 100%;
}

i {
  font-size: 1rem !important;
}

.table tr {
  border-radius: 20px;
  border: gray;
}

tr td:nth-child(n + 10),
tr th:nth-child(n + 10) {
  border-radius: 0 0.625rem 0.625rem 0;
}

tr td:nth-child(n + 12),
tr th:nth-child(n + 12) {
  border-radius: 0.625rem 0 0 0.625rem;
}

.fade-enter {
  opacity: 0.5;
  transition: 0.5s all ease;
}

.fade-enter-to {
  opacity: 1;
  transition: 0.5s all ease-in;
}

.fade-leave-to {
  opacity: 0.2;
  transition: 0.2s all ease-in-out;
}

.mapboxgl-canvas {
  width: 100% !important;
  height: 500px !important;
}
</style>
