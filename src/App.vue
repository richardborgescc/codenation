<template>
  <div id="app">
    <transition name="fade">
      <router-view v-if="show" />
    </transition>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      show: false
    };
  },
  mounted() {
    this.show = true;
  }
};
</script>

<style lang="scss">
@import "@/theme/bulma.scss";

.fade-enter-active {
  transition: opacity 0.3s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
