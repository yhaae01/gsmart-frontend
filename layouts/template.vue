<template>
  <div>
    <div class="app-page flex-column flex-column-fluid" id="kt_app_page">
      <AppHeader />
      <hr class="mt-0 mb-0" />
      <div class="app-wrapper flex-column flex-row-fluid" id="kt_app_wrapper">
        <AppSidebar />
        <div
          class="app-main flex-column flex-row-fluid bg-light"
          id="kt_app_main"
        >
          <nuxt />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppHeader from '~/components/header.vue'
import AppSidebar from '~/components/sidebar.vue'
export default {
  layout: 'default',
  components: { AppHeader, AppSidebar },
}
</script>
