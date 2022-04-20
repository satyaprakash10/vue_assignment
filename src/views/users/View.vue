<template>
  <div class="px-4 py-4 mb-10 sm:px-0 sm:py-0">
    <!-- User breadcrumb  section -->
    <div class="">
      <div
        class="w-full max-w-5xl px-4 py-3 mt-10 bg-white border-t-4 border-green-500 rounded-md shadow-lg"
      >
        <nav class="" aria-label="Breadcrumb">
          <div class="flex items-center justify-between">
            <div>
              <h1
                class="flex items-center font-sans text-xl font-bold sm:text-2xl"
              >
                <img
                  v-if="selectedUser.image"
                  :src="selectedUser.image"
                  class="w-10 h-10 rounded-full shadow-md"
                  alt="user_image"
                />

                <img
                  v-else
                  src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
                  class="w-10 h-10 rounded-full shadow-md"
                  alt="user_image"
                />
                <span v-if="selectedUser.name" class="ml-4">
                  {{ selectedUser.name }}
                </span>
              </h1>
            </div>
          </div>

          <nav class="flex mt-6 mb-2" aria-label="Breadcrumb">
            <ol role="list" class="flex items-center space-x-4">
              <li>
                <div>
                  <router-link to="/" class="text-gray-400 hover:text-gray-500">
                    <HomeIcon
                      class="flex-shrink-0 w-5 h-5"
                      aria-hidden="true"
                    />
                    <span class="sr-only">Home</span>
                  </router-link>
                </div>
              </li>
              <li>
                <div class="flex items-center">
                  <ChevronRightIcon
                    class="flex-shrink-0 w-5 h-5 text-gray-400"
                    aria-hidden="true"
                  />
                  <router-link
                    to="/"
                    class="flex items-center ml-4 text-sm font-medium text-gray-500 hover:text-gray-700"
                  >
                    <UsersIcon
                      class="flex-shrink-0 w-5 h-5 mr-3"
                      aria-hidden="true"
                    />
                    Users
                  </router-link>
                </div>
              </li>
              <li>
                <div class="flex items-center">
                  <ChevronRightIcon
                    class="flex-shrink-0 w-5 h-5 text-gray-500"
                    aria-hidden="true"
                  />
                  <router-link
                    to="#"
                    class="flex items-center ml-4 text-sm font-semibold text-green-500 hover:text-green-600"
                  >
                    <UserIcon
                      class="flex-shrink-0 w-5 h-5 mr-3"
                      aria-hidden="true"
                    />
                    {{ $route.params.id ? 'View' : 'New' }}
                    <span v-if="selectedUser.name">
                      {{ selectedUser.name }}
                    </span>
                  </router-link>
                </div>
              </li>
            </ol>
          </nav>
        </nav>
      </div>
    </div>

    <!-- User info card -->
    <h1 class="py-4 mt-4 text-lg font-bold text-gray-500 sm:text-2xl">
      User Information
    </h1>

    <div
      v-if="selectedUser"
      class="px-4 py-4 mx-auto mt-4 bg-white border-t-4 border-green-500 rounded-md shadow-lg"
      max-width="800"
    >
      <template slot="progress">
        <v-progress-linear
          color="deep-purple"
          height="10"
          indeterminate
        ></v-progress-linear>
      </template>

      <div class="grid grid-cols-1 gap-4 sm:grid-cols-2">
        <div class="block sm:hidden lg:hidden">
          <img
            v-if="selectedUser.image"
            :src="selectedUser.image"
            class="w-full h-full rounded-lg shadow-md lg:w-40 lg:h-40"
            alt="user_image"
          />

          <img
            v-else
            src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
            class="w-full h-full rounded-lg shadow-md lg:w-40 lg:h-40"
            alt="user_image"
          />
        </div>

        <div
          class="grid col-span-2 px-4 py-2 border border-green-500 rounded-md"
        >
          <div
            class="grid grid-cols-1 mt-4 text-base font-bold text-gray-900 sm:grid-cols-3 sm:gap-4 sm:text-xl"
          >
            <label for="" class="text-base font-medium text-gray-500">
              Name:
            </label>
            {{ selectedUser.name }}
          </div>
          <div
            class="grid grid-cols-1 mt-4 text-base font-bold text-gray-900 sm:grid-cols-3 sm:gap-4 sm:text-xl"
          >
            <label for="" class="text-base font-medium text-gray-500">
              Email:
            </label>
            {{ selectedUser.email }}
          </div>
          <div
            class="grid grid-cols-1 mt-4 text-base font-bold text-gray-900 sm:grid-cols-3 sm:gap-4 sm:text-xl"
          >
            <label for="" class="text-base font-medium text-gray-500">
              Phone:
            </label>
            {{ selectedUser.mobile }}
          </div>
          <div
            class="grid grid-cols-1 mt-4 mb-4 text-base font-bold text-gray-900 sm:grid-cols-3 sm:gap-4 sm:text-xl"
          >
            <label for="" class="text-base font-medium text-gray-500">
              Address:
            </label>
            {{ selectedUser.address }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { HomeIcon, ChevronRightIcon } from '@vue-hero-icons/solid'
import axios from 'axios'
export default {
  components: {
    HomeIcon,
    ChevronRightIcon,
  },

  data() {
    return {
      selectedUser: null,
    }
  },

  mounted() {
    if (this.$route.params.id) {
      this.fetchData()
    }
  },
  methods: {
    fetchData() {
      axios
        .get(`http://localhost:3000/users/${this.$route.params.id}`)
        .then((res) => {
          this.selectedUser = res.data
        })
    },
  },
}
</script>
