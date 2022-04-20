<template>
  <div class="mb-10">
    <!-- User breadcrumb  Section -->
    <div class="pl-3">
      <div
        class="w-full max-w-5xl px-4 py-3 mt-10 bg-white border-t-4 border-green-500 rounded-md shadow-lg"
      >
        <nav class="" aria-label="Breadcrumb">
          <div class="flex items-center justify-between">
            <div>
              <h1 class="font-sans text-xl font-bold sm:text-2xl">
                {{ $route.params.id ? 'Edit' : 'New' }}

                <span v-if="formData">
                  {{ formData.name }}
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
                    {{ $route.params.id ? 'Edit User' : 'New User' }}
                  </router-link>
                </div>
              </li>
            </ol>
          </nav>
        </nav>
      </div>
    </div>

    <hr class="w-full mx-auto mt-10 border-green-300 max-w-7xl" />

    <div class="w-full mt-10">
      <div
        class="px-4 py-5 mx-4 bg-white border-t-4 border-green-500 rounded-md shadow-lg"
        max-width="1450"
      >
        <form action="" @submit.prevent="submitUserData">
          <div
            @change="uploadImage"
            class="flex justify-center px-6 pt-5 pb-6 mt-2 border-2 border-gray-300 border-dashed rounded-md cursor-pointer hover:bg-gray-200 mb-7"
          >
            <div class="space-y-1 text-center">
              <svg
                v-if="!previewImage"
                class="w-12 h-12 mx-auto text-gray-400"
                stroke="currentColor"
                fill="none"
                viewBox="0 0 48 48"
                aria-hidden="true"
              >
                <path
                  d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              <div class="flex text-sm text-gray-600">
                <label
                  for="file-upload"
                  class="relative font-medium text-indigo-600 bg-white rounded-md cursor-pointer hover:text-indigo-500 focus-within:outline-none focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-indigo-500"
                >
                  <span v-if="!previewImage">Upload a file</span>
                  <input
                    id="file-upload"
                    name="image"
                    type="file"
                    class="sr-only"
                    @change="uploadImage"
                  />
                  <img
                    v-if="previewImage"
                    :src="previewImage"
                    alt=""
                    class="relative w-full h-40 cursor-pointer"
                  />
                </label>
                <div
                  v-if="previewImage"
                  @click="removeImage"
                  class="absolute p-2 font-black text-white bg-green-500 rounded-md shadow-sm cursor-pointer bfont-semibold bg-opacity-70 bottom-96 hover:bg-green-500 hover:bg-opacity-40 hover:text-red w-36"
                >
                  Remove
                </div>
                <p v-if="!previewImage" class="pl-1">or drag and drop</p>
              </div>
            </div>
          </div>
          <v-text-field
            v-model="formData.name"
            :counter="10"
            :error-messages="errors"
            label="Name"
            required
            @input="$v.formData.name.$touch()"
            @blur="$v.formData.name.$touch()"
          ></v-text-field>

          <v-text-field
            v-model="formData.email"
            :error-messages="emailErrors"
            label="E-mail"
            required
            @input="$v.formData.email.$touch()"
            @blur="$v.formData.email.$touch()"
          ></v-text-field>

          <v-text-field
            v-model="formData.mobile"
            :error-messages="mobileErrors"
            label="Mobile"
            required
            @input="$v.formData.email.$touch()"
            @blur="$v.formData.email.$touch()"
          ></v-text-field>

          <div id="geocoder" class="mb-8"></div>
          <pre id="result" ref="result" class="hidden"></pre>

          <button
            type="submit"
            class="inline-flex items-center px-4 py-3 text-sm font-semibold leading-4 text-white transition-all duration-500 ease-linear bg-green-500 border border-transparent rounded-md shadow-sm group hover:shadow-lg hover:bg-opacity-60 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            <SaveIcon
              class="w-6 h-6 mr-3 text-white group-hover:text-green-600"
            />
            {{ $route.params.id ? 'Update User' : 'Save User' }}
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import { validationMixin } from 'vuelidate'
import { required, email } from 'vuelidate/lib/validators'
import { mapActions, mapGetters } from 'vuex'
import {
  SaveIcon,
  HomeIcon,
  UsersIcon,
  ChevronRightIcon,
  UserIcon,
} from '@vue-hero-icons/solid'
export default {
  mixins: [validationMixin],

  components: {
    HomeIcon,
    SaveIcon,
    ChevronRightIcon,
    UsersIcon,
    UserIcon,
  },

  data() {
    return {
      previewImage: '',
      formData: {
        id: null,
        image: '',
        name: '',
        email: '',
        mobile: '',
        coordinates: '',
        address: '',
      },
    }
  },

  validations: {
    formData: {
      name: { required },
      email: { required, email },
      mobile: { required },
    },
  },

  computed: {
    ...mapGetters(['getSelectedUser']),

    nameErrors() {
      const errors = []
      if (!this.$v.formData.name.$dirty) return errors
      !this.$v.formData.name.required && errors.push('Name is required.')
      return errors
    },

    emailErrors() {
      const errors = []
      if (!this.$v.formData.email.$dirty) return errors
      !this.$v.formData.email.email && errors.push('Must be valid e-mail')
      !this.$v.formData.email.required && errors.push('E-mail is required')
      return errors
    },

    mobileErrors() {
      const errors = []
      if (!this.$v.formData.mobile.$dirty) return errors
      !this.$v.formData.mobile.required && errors.push('Mobile is required')
      return errors
    },
  },

  mounted() {
    this.initAddress()
    if (this.$route.params.id) {
      this.getUserData()
    }
  },

  methods: {
    ...mapActions(['addUser', 'updateUser']),

    getUserData() {
      axios
        .get(`http://localhost:3000/users/${this.$route.params.id}`)
        .then((response) => {
          if (response) {
            this.previewImage = response.data.image
            this.formData.image = response.data.image
            this.formData.name = response.data.name
            this.formData.email = response.data.email
            this.formData.mobile = response.data.mobile
            this.formData.address = response.data.address
            this.formData.coordinates = response.data.coordinates
          }
        })
    },

    uploadImage(event) {
      const input = event.target
      if (input.files) {
        let reader = new FileReader()
        reader.onload = (e) => {
          this.previewImage = e.target.result
          this.formData.image = e.target.result
        }
        reader.readAsDataURL(input.files[0])
      }
    },

    removeImage() {
      this.previewImage = ''
      this.formData.image = ''
    },

    initAddress() {
      mapboxgl.accessToken =
        'pk.eyJ1IjoiaGFzaWkiLCJhIjoiY2wwYWt4cjJ1MDF4YTNjbWptNzZwM2ZhZyJ9.9WUVDkgiL5lsqX95u0VAvA'

      const geocoder = new MapboxGeocoder({
        accessToken: mapboxgl.accessToken,
        types: 'country,region,place,postcode,locality,neighborhood',
      })

      geocoder.addTo('#geocoder')

      // Get the geocoder results container.
      const results = document.getElementById('result')

      // Add geocoder result to container.
      geocoder.on('result', (e) => {
        results.innerText = JSON.stringify(e.result, null, 2)
        this.formData.coordinates = e.result.geometry.coordinates
        this.formData.address = e.result.place_name
      })

      // Clear results container when search is cleared.
      geocoder.on('clear', () => {
        results.innerText = ''
      })
    },

    submitUserData() {
      this.$v.$touch()
      if (this.$route.params.id) {
        this.updateUser({
          id: this.$route.params.id,
          data: this.formData,
        }).then(() => {
          this.$toastr.s('User updated successfully')
          this.$router.push('/')
        })
      } else {
        this.addUser(this.formData).then(() => {
          this.$toastr.s('User added successfully')
          this.$router.push('/')
        })
      }
    },
  },
}
</script>

<style>
.mapboxgl-ctrl-geocoder.mapboxgl-ctrl {
  width: 100%;
  max-width: 900px;
  border: 1px solid gray;
}
</style>
