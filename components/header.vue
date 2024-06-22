<template>
  <div>
    <template>
      <div>
        <!--begin::Header-->
        <div id="kt_app_header" class="app-header">
          <!--begin::Header container-->
          <div
            class="
              app-container
              container-fluid
              d-flex
              align-items-stretch
              justify-content-between
            "
          >
            <!--begin::sidebar mobile toggle-->
            <div
              class="d-flex align-items-center d-lg-none ms-n2 me-2"
              title="Show sidebar menu"
            >
              <div
                class="btn btn-icon btn-active-color-primary w-35px h-35px"
                id="kt_app_sidebar_mobile_toggle"
              >
                <!--begin::Svg Icon | path: icons/duotune/abstract/abs015.svg-->
                <span class="svg-icon svg-icon-1">
                  <svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      d="M21 7H3C2.4 7 2 6.6 2 6V4C2 3.4 2.4 3 3 3H21C21.6 3 22 3.4 22 4V6C22 6.6 21.6 7 21 7Z"
                      fill="currentColor"
                    />
                    <path
                      opacity="0.3"
                      d="M21 14H3C2.4 14 2 13.6 2 13V11C2 10.4 2.4 10 3 10H21C21.6 10 22 10.4 22 11V13C22 13.6 21.6 14 21 14ZM22 20V18C22 17.4 21.6 17 21 17H3C2.4 17 2 17.4 2 18V20C2 20.6 2.4 21 3 21H21C21.6 21 22 20.6 22 20Z"
                      fill="currentColor"
                    />
                  </svg>
                </span>
                <!--end::Svg Icon-->
              </div>
            </div>
            <!--end::sidebar mobile toggle-->
            <!--begin::Mobile logo-->
            <div class="d-flex align-items-center flex-grow-1 flex-lg-grow-0">
              <a href="#" class="d-lg-none">
                <h5>G-SMART</h5>
              </a>
            </div>
            <!--end::Mobile logo-->
            <!--begin::Header wrapper-->
            <div
              class="
                d-flex
                align-items-stretch
                justify-content-between
                flex-lg-grow-1
              "
              id="kt_app_header_wrapper"
            >
              <!--begin::Menu wrapper-->
              <div
                class="
                  app-header-menu app-header-mobile-drawer
                  align-items-stretch
                "
                data-kt-drawer="true"
                data-kt-drawer-name="app-header-menu"
                data-kt-drawer-activate="{default: true, lg: false}"
                data-kt-drawer-overlay="true"
                data-kt-drawer-width="225px"
                data-kt-drawer-direction="end"
                data-kt-drawer-toggle="#kt_app_header_menu_toggle"
                data-kt-swapper="true"
                data-kt-swapper-mode="{default: 'append', lg: 'prepend'}"
                data-kt-swapper-parent="{default: '#kt_app_body', lg: '#kt_app_header_wrapper'}"
              >
                <!--begin::Menu-->
                <div
                  class="
                    menu menu-rounded menu-column menu-lg-row
                    my-5 my-lg-0
                    align-items-stretch
                    fw-semibold
                    px-2 px-lg-0
                  "
                  id="kt_app_header_menu"
                  data-kt-menu="true"
                ></div>
                <!--end::Menu-->
              </div>
              <!--end::Menu wrapper-->
              <!--begin::Navbar-->
              <div class="app-navbar flex-shrink-0">
                <div
                  class="app-navbar-item ms-1 ms-lg-3"
                  id="kt_header_user_menu_toggle"
                >
                  <p class="text-dark mb-0">Hi, {{ user }}</p>
                </div>
                <!--begin::User menu-->
                <div
                  class="app-navbar-item ms-1 ms-lg-3"
                  id="kt_header_user_menu_toggle"
                >
                  <!--begin::Menu wrapper-->
                  <div
                    class="cursor-pointer symbol symbol-35px symbol-md-40px"
                    data-kt-menu-trigger="click"
                    data-kt-menu-attach="parent"
                    data-kt-menu-placement="bottom-end"
                  >
                    <img :src="photo" @error="setAltImg" alt="user" />
                  </div>
                  <!--begin::User account menu-->
                  <div
                    class="
                      menu
                      menu-sub
                      menu-sub-dropdown
                      menu-column
                      menu-rounded
                      menu-gray-800
                      menu-state-bg
                      menu-state-primary
                      fw-semibold
                      py-4
                      fs-6
                      w-275px
                    "
                    data-kt-menu="true"
                  >
                    <!--begin::Menu item-->
                    <div class="menu-item px-3">
                      <div class="menu-content d-flex align-items-center px-3">
                        <!--begin::Avatar-->
                        <div class="symbol symbol-50px me-5">
                          <img
                            alt="Logo"
                            :src="photo"
                            @error="setAltImg"
                          />
                        </div>
                        <!--end::Avatar-->
                        <!--begin::Username-->
                        <div class="d-flex flex-column">
                          <div class="fw-bold d-flex align-items-center fs-5">
                            {{ user }}
                          </div>
                          <a
                            href="#"
                            class="
                              fw-semibold
                              text-muted text-hover-primary
                              fs-7
                            "
                            >{{ email }}</a
                          >
                        </div>
                        <!--end::Username-->
                      </div>
                    </div>
                    <!--end::Menu item-->
                    <!--begin::Menu separator-->
                    <div class="separator my-2"></div>
                    <!--end::Menu separator-->
                    <!--begin::Menu item-->
                    <div class="menu-item px-5">
                      <a v-on:click.prevent="logout" class="menu-link px-5"
                        >Sign Out</a
                      >
                    </div>
                    <!--end::Menu item-->
                  </div>
                  <!--end::User account menu-->
                  <!--end::Menu wrapper-->
                </div>
                <!--end::User menu-->
              </div>
              <!--end::Navbar-->
            </div>
            <!--end::Header wrapper-->
          </div>
          <!--end::Header container-->
        </div>
        <!--end::Header-->
      </div>
    </template>
  </div>
</template>

<script>
export default {
  mounted() {
    KTMenu.createInstances()
    KTDrawer.createInstances()
    KTScroll.createInstances()
    KTScrolltop.createInstances()
    KTSticky.createInstances()
    KTImageInput.createInstances()
    KTPasswordMeter.createInstances()
    KTSwapper.createInstances()
    KTToggle.createInstances()
  },
  head() {
    return {
      bodyAttrs: {
        class: 'app-default',
        id: 'kt_app_body',
        data_kt_name: 'metronic',
        data_kt_app_layout: 'dark-sidebar',
        data_kt_app_header_fixed: 'true',
        data_kt_app_sidebar_enabled: 'true',
        data_kt_app_sidebar_fixed: 'true',
        data_kt_app_sidebar_hoverable: 'true',
        data_kt_app_sidebar_push_header: 'true',
        data_kt_app_sidebar_push_toolbar: 'true',
        data_kt_app_sidebar_push_footer: 'true',
        data_kt_app_toolbar_enabled: 'true',
        data_theme: 'light',
      },
    }
  },
  data() {
    return {
      user: this.$auth.user.name,
      email: this.$auth.user.email,
      photo: this.$auth.user.photo,
    }
  },
  methods: {
    setAltImg(event) { 
      event.target.src = "https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460_960_720.png" 
    }, 
    async logout() {
      await this.$auth.logout()
      this.loading()
      this.$router.push('/login')
      this.successMessage()
    },
    loading() {
      Swal.fire({
        timer: 500,
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
      })
    },
    successMessage() {
      toastr.options = {
        closeButton: false,
        debug: false,
        newestOnTop: false,
        progressBar: false,
        positionClass: 'toastr-top-right',
        preventDuplicates: false,
        onclick: null,
        showDuration: '300',
        hideDuration: '1000',
        timeOut: '5000',
        extendedTimeOut: '1000',
        showEasing: 'swing',
        hideEasing: 'linear',
        showMethod: 'fadeIn',
        hideMethod: 'fadeOut',
      }
      toastr.success('Logout Successfully!')
    },
  },
}
</script>

<style>
</style>
