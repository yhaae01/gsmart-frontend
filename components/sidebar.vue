<template>
  <div>
    <!--begin::sidebar-->
    <div
      id="kt_app_sidebar"
      class="app-sidebar flex-column"
      data-kt-drawer="true"
      data-kt-drawer-name="app-sidebar"
      data-kt-drawer-activate="{default: true, lg: false}"
      data-kt-drawer-overlay="true"
      data-kt-drawer-width="225px"
      data-kt-drawer-direction="start"
      data-kt-drawer-toggle="#kt_app_sidebar_mobile_toggle"
    >
      <!--begin::Logo-->
      <div
        class="app-sidebar-logo px-6 border-bottom-0"
        id="kt_app_sidebar_logo"
      >
        <!--begin::Logo image-->
        <a href="#">
          <h1 class="h-20px app-sidebar-logo-default text-white">G-SMART</h1>
          <h1 class="h-10px app-sidebar-logo-minimize text-white">G-</h1>
        </a>
        <!--end::Logo image-->
        <!--begin::Sidebar toggle-->
        <div
          id="kt_app_sidebar_toggle"
          class="app-sidebar-toggle btn btn-icon btn-shadow btn-sm btn-color-muted btn-active-color-primary body-bg h-30px w-30px position-absolute top-50 start-100 translate-middle rotate"
          data-kt-toggle="true"
          data-kt-toggle-state="active"
          data-kt-toggle-target="body"
          data-kt-toggle-name="app-sidebar-minimize"
        >
          <!--begin::Svg Icon | path: icons/duotune/arrows/arr079.svg-->
          <span class="svg-icon svg-icon-2 rotate-180">
            <svg
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                opacity="0.5"
                d="M14.2657 11.4343L18.45 7.25C18.8642 6.83579 18.8642 6.16421 18.45 5.75C18.0358 5.33579 17.3642 5.33579 16.95 5.75L11.4071 11.2929C11.0166 11.6834 11.0166 12.3166 11.4071 12.7071L16.95 18.25C17.3642 18.6642 18.0358 18.6642 18.45 18.25C18.8642 17.8358 18.8642 17.1642 18.45 16.75L14.2657 12.5657C13.9533 12.2533 13.9533 11.7467 14.2657 11.4343Z"
                fill="currentColor"
              />
              <path
                d="M8.2657 11.4343L12.45 7.25C12.8642 6.83579 12.8642 6.16421 12.45 5.75C12.0358 5.33579 11.3642 5.33579 10.95 5.75L5.40712 11.2929C5.01659 11.6834 5.01659 12.3166 5.40712 12.7071L10.95 18.25C11.3642 18.6642 12.0358 18.6642 12.45 18.25C12.8642 17.8358 12.8642 17.1642 12.45 16.75L8.2657 12.5657C7.95328 12.2533 7.95328 11.7467 8.2657 11.4343Z"
                fill="currentColor"
              />
            </svg>
          </span>
        </div>
        <!--end::Sidebar toggle-->
      </div>
      <!--end::Logo-->
      <!--begin::sidebar menu-->
      <div class="app-sidebar-menu overflow-hidden flex-column-fluid">
        <!--begin::Menu wrapper-->
        <div
          id="kt_app_sidebar_menu_wrapper"
          class="app-sidebar-wrapper hover-scroll-overlay-y my-5"
          data-kt-scroll="true"
          data-kt-scroll-activate="true"
          data-kt-scroll-height="auto"
          data-kt-scroll-dependencies="#kt_app_sidebar_logo, #kt_app_sidebar_footer"
          data-kt-scroll-wrappers="#kt_app_sidebar_menu"
          data-kt-scroll-offset="5px"
          data-kt-scroll-save-state="true"
        >
          <!--begin::Menu-->
          <div
            class="menu menu-column menu-rounded menu-sub-indention px-3"
            id="#kt_app_sidebar_menu"
            data-kt-menu="true"
            data-kt-menu-expand="false"
          >
            <div class="menu-item">
              <NuxtLink class="menu-link" to="/" exact-active-class="active">
                <span class="menu-icon">
                  <img
                    class="h-20px w-20px"
                    src="~/assets/media/icons/duotune/abstract/abs027.svg"
                  />
                </span>
                <span class="menu-title">Dashboards</span>
              </NuxtLink>
            </div>
            <div class="menu-item" v-if="role !== 'Initial'">
              <NuxtLink
                v-if="role !== 'CBO'"
                class="menu-link"
                to="/my-prospect"
                active-class="active"
              >
                <span class="menu-icon">
                  <i class="fa-solid fa-briefcase"></i>
                </span>
                <span class="menu-title">Prospect</span>
              </NuxtLink>
            </div>
            <div
              class="menu-item"
              v-if="
                role == 'TPR' ||
                role == 'TPC' ||
                role == 'CBO' ||
                role == 'AMS' ||
                role == 'AM' ||
                role == 'Administrator'
              "
            >
              <NuxtLink
                class="menu-link"
                to="/my-salesplan"
                active-class="active"
              >
                <span class="menu-icon">
                  <img
                    class="h-20px w-20px"
                    src="~/assets/media/icons/duotune/general/gen022.svg"
                  />
                </span>
                <span
                  class="menu-title"
                  v-if="
                    role == 'TPR' ||
                    role == 'TPC' ||
                    role == 'CBO' ||
                    role == 'Administrator'
                  "
                  >Sales Plan</span
                >
                <span class="menu-title" v-if="role == 'AMS' || role == 'AM'"
                  >Sales Plan</span
                >
              </NuxtLink>
            </div>

            <div v-if="role == 'Administrator'">
              <div
                data-kt-menu-trigger="click"
                class="menu-item menu-accordion"
                active-class="hover show"
              >
                <span class="menu-link">
                  <span class="menu-icon">
                    <img
                      class="h-20px w-20px"
                      src="~/assets/media/icons/duotune/general/gen025.svg"
                    />
                  </span>
                  <span class="menu-title">Master Data</span>
                  <span class="menu-arrow"></span>
                </span>
                <div class="menu-sub menu-sub-accordion" active-class="show">
                  <div class="menu-item">
                    <!-- <Nuxt-link
                      class="menu-link"
                      to="/customer"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">AMS Customers</span>
                    </Nuxt-link> -->
                    <Nuxt-link
                      class="menu-link"
                      to="/customer-group"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Customer Group</span>
                    </Nuxt-link>
                    <Nuxt-link class="menu-link" to="/am" active-class="active">
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">AM</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/ams"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">AMS</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/country"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Country</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/region"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Region</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/area"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Area</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/user"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">User</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/role"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Role</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/permission"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Permission</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/transaction-type"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Transaction Type</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/prospect-type"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Prospect Type</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/strategic-initiative"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Strategic Initiative</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/product"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Product</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/aircraft-type"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Aircraft Type</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/component"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Component</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/engine"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Engine</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/apu"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">APU</span>
                    </Nuxt-link>
                    <Nuxt-link
                      class="menu-link"
                      to="/maintenance"
                      active-class="active"
                    >
                      <span class="menu-bullet">
                        <span class="bullet bullet-dot"></span>
                      </span>
                      <span class="menu-title">Maintenance</span>
                    </Nuxt-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!--end::Menu-->
        </div>
        <!--end::Menu wrapper-->
      </div>
      <!--end::sidebar menu-->
    </div>
    <!--end::sidebar-->
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: this.$auth.user.name,
      email: this.$auth.user.email,
      role: this.$auth.user.role.name,
    }
  },
}
</script>

<style></style>
