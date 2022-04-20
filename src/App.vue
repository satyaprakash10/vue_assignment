<template>
  <div
    id="app"
    class="relative w-full overflow-auto"
    style="height: 100vh; background: #fef2f2"
  >
    <!-- the site header -->

    <TheSiteHeader class="hidden" />

    <!-- Main content -->
    <div class="relative w-full mx-auto max-w-7xl">
      <!--  pages route  -->
      <transition name="fade">
        <router-view />
      </transition>
    </div>
  </div>
</template>

<script>
import TheSiteHeader from './components/TheSiteHeader.vue'

export default {
  components: {
    TheSiteHeader,
  },

  data() {
    return {}
  },
}
</script>

<style scoped>
.body {
  height: 100vh;
}

.fade-enter {
  opacity: 0.5;
  transition: 0.5s all ease;
}

.fade-enter-to {
  opacity: 1;
  transition: 0.5s all ease;
}

.fade-leave-to {
  opacity: 0;
  transition: 0.5s all ease-out;
}
</style>
