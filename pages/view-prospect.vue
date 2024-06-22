<template>
  <div>
    <div id="kt_app_toolbar" class="app-toolbar py-3 py-lg-6 bg-white">
      <!--begin::Toolbar container-->
      <div
        id="kt_app_toolbar_container"
        class="app-container container-fluid d-flex flex-stack"
      >
        <!--begin::Page title-->
        <div
          class="
            page-title
            d-flex
            flex-column
            justify-content-center
            flex-wrap
            me-3
          "
        >
          <!--begin::Title-->
          <p
            class="
              page-heading
              d-flex
              text-dark
              fs-6
              flex-column
              justify-content-center
              my-0
            "
          >
            View Prospect
          </p>
          <!--end::Title-->
        </div>
        <ul
          class="breadcrumb breadcrumb-separatorless fw-semibold fs-7 my-0 pt-1"
        >
          <!--begin::Item-->
          <li class="breadcrumb-item text-muted">
            <nuxt-link to="/my-prospect" class="text-muted text-hover-primary"
              >My Prospect</nuxt-link
            >
          </li>
          <li class="breadcrumb-item">
            <span class="bullet bg-gray-400 w-5px h-2px"></span>
          </li>
          <li class="breadcrumb-item text-muted">View Prospect</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="row">
          <div class="col-md-12 p-16">
            <div class="row mt-15 mb-2">
              <h2 class="mb-0">
                {{ customer.name }}
                <span class="badge bg-white">
                  <span class="svg-icon svg-icon-primary svg-icon-2hx mt-0"
                    ><svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="24px"
                      height="24px"
                      viewBox="0 0 24 24"
                    >
                      <path
                        d="M10.0813 3.7242C10.8849 2.16438 13.1151 2.16438 13.9187 3.7242V3.7242C14.4016 4.66147 15.4909 5.1127 16.4951 4.79139V4.79139C18.1663 4.25668 19.7433 5.83365 19.2086 7.50485V7.50485C18.8873 8.50905 19.3385 9.59842 20.2758 10.0813V10.0813C21.8356 10.8849 21.8356 13.1151 20.2758 13.9187V13.9187C19.3385 14.4016 18.8873 15.491 19.2086 16.4951V16.4951C19.7433 18.1663 18.1663 19.7433 16.4951 19.2086V19.2086C15.491 18.8873 14.4016 19.3385 13.9187 20.2758V20.2758C13.1151 21.8356 10.8849 21.8356 10.0813 20.2758V20.2758C9.59842 19.3385 8.50905 18.8873 7.50485 19.2086V19.2086C5.83365 19.7433 4.25668 18.1663 4.79139 16.4951V16.4951C5.1127 15.491 4.66147 14.4016 3.7242 13.9187V13.9187C2.16438 13.1151 2.16438 10.8849 3.7242 10.0813V10.0813C4.66147 9.59842 5.1127 8.50905 4.79139 7.50485V7.50485C4.25668 5.83365 5.83365 4.25668 7.50485 4.79139V4.79139C8.50905 5.1127 9.59842 4.66147 10.0813 3.7242V3.7242Z"
                        fill="currentColor"
                      />
                      <path
                        d="M14.8563 9.1903C15.0606 8.94984 15.3771 8.9385 15.6175 9.14289C15.858 9.34728 15.8229 9.66433 15.6185 9.9048L11.863 14.6558C11.6554 14.9001 11.2876 14.9258 11.048 14.7128L8.47656 12.4271C8.24068 12.2174 8.21944 11.8563 8.42911 11.6204C8.63877 11.3845 8.99996 11.3633 9.23583 11.5729L11.3706 13.4705L14.8563 9.1903Z"
                        fill="white"
                      /></svg
                  ></span>
                </span>
              </h2>
            </div>
            <div class="row">
              <div class="col-sm-2">
                <span class="svg-icon svg-icon-muted svg-icon-2"
                  ><svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      d="M6.28548 15.0861C7.34369 13.1814 9.35142 12 11.5304 12H12.4696C14.6486 12 16.6563 13.1814 17.7145 15.0861L19.3493 18.0287C20.0899 19.3618 19.1259 21 17.601 21H6.39903C4.87406 21 3.91012 19.3618 4.65071 18.0287L6.28548 15.0861Z"
                      fill="currentColor"
                    />
                    <rect
                      opacity="0.3"
                      x="8"
                      y="3"
                      width="8"
                      height="8"
                      rx="4"
                      fill="currentColor"
                    />
                  </svg>
                  <span class="badge bg-white text-muted fs-6 mb-0 p-0">{{ customer.code }}</span>
                </span>
              </div>
              <div class="col-sm-2">
                <span class="svg-icon svg-icon-muted svg-icon-2"
                  ><svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      opacity="0.3"
                      d="M18.0624 15.3453L13.1624 20.7453C12.5624 21.4453 11.5624 21.4453 10.9624 20.7453L6.06242 15.3453C4.56242 13.6453 3.76242 11.4453 4.06242 8.94534C4.56242 5.34534 7.46242 2.44534 11.0624 2.04534C15.8624 1.54534 19.9624 5.24534 19.9624 9.94534C20.0624 12.0453 19.2624 13.9453 18.0624 15.3453Z"
                      fill="currentColor"
                    />
                    <path
                      d="M12.0624 13.0453C13.7193 13.0453 15.0624 11.7022 15.0624 10.0453C15.0624 8.38849 13.7193 7.04535 12.0624 7.04535C10.4056 7.04535 9.06241 8.38849 9.06241 10.0453C9.06241 11.7022 10.4056 13.0453 12.0624 13.0453Z"
                      fill="currentColor"
                    />
                  </svg>
                  <span class="badge bg-white text-muted fs-6 mb-0 p-0">
                    {{ customer_area.name }}
                  </span>
                </span>
              </div>
              <div class="col-sm-5">
                <span class="svg-icon svg-icon-muted svg-icon-2"
                  ><svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      opacity="0.3"
                      d="M5.78001 21.115L3.28001 21.949C3.10897 22.0059 2.92548 22.0141 2.75004 21.9727C2.57461 21.9312 2.41416 21.8418 2.28669 21.7144C2.15923 21.5869 2.06975 21.4264 2.0283 21.251C1.98685 21.0755 1.99507 20.892 2.05201 20.7209L2.886 18.2209L7.22801 13.879L10.128 16.774L5.78001 21.115Z"
                      fill="currentColor"
                    />
                    <path
                      d="M21.7 8.08899L15.911 2.30005C15.8161 2.2049 15.7033 2.12939 15.5792 2.07788C15.455 2.02637 15.3219 1.99988 15.1875 1.99988C15.0531 1.99988 14.92 2.02637 14.7958 2.07788C14.6717 2.12939 14.5589 2.2049 14.464 2.30005L13.74 3.02295C13.548 3.21498 13.4402 3.4754 13.4402 3.74695C13.4402 4.01849 13.548 4.27892 13.74 4.47095L14.464 5.19397L11.303 8.35498C10.1615 7.80702 8.87825 7.62639 7.62985 7.83789C6.38145 8.04939 5.2293 8.64265 4.332 9.53601C4.14026 9.72817 4.03256 9.98855 4.03256 10.26C4.03256 10.5315 4.14026 10.7918 4.332 10.984L13.016 19.667C13.208 19.859 13.4684 19.9668 13.74 19.9668C14.0115 19.9668 14.272 19.859 14.464 19.667C15.3575 18.77 15.9509 17.618 16.1624 16.3698C16.374 15.1215 16.1932 13.8383 15.645 12.697L18.806 9.53601L19.529 10.26C19.721 10.452 19.9814 10.5598 20.253 10.5598C20.5245 10.5598 20.785 10.452 20.977 10.26L21.7 9.53601C21.7952 9.44108 21.8706 9.32825 21.9221 9.2041C21.9737 9.07995 22.0002 8.94691 22.0002 8.8125C22.0002 8.67809 21.9737 8.54505 21.9221 8.4209C21.8706 8.29675 21.7952 8.18392 21.7 8.08899Z"
                      fill="currentColor"
                    />
                  </svg>
                  <span class="badge bg-white text-muted fs-6 mb-0 p-0">
                  {{ customer_country.name }} - {{ customer_region.name }}
                  </span>
                </span>
              </div>
            </div>
            <div class="row mt-5 pb-15">
              <div class="col-3">
                <div class="border-dashed rounded p-4">
                  <h1 class="fw-bolder mb-0" v-if="prospect_total.marketShare">${{ formatNumber(prospect_total.marketShare) }}</h1>
                  <h1 class="fw-bolder mb-0" v-else>$ 0</h1>
                  <p class="mb-0 fw-bold text-gray-500">Total Market Share</p>
                </div>
              </div>
              <div class="col-3">
                <div class="border-dashed rounded p-4">
                  <h1 class="fw-bolder mb-0" v-if="prospect_total.salesPlan">${{ formatNumber(prospect_total.salesPlan) }}</h1>
                  <h1 class="fw-bolder mb-0" v-else>$ 0 </h1>
                  <p class="mb-0 fw-bold text-gray-500">Total Salesplan</p>
                </div>
              </div>
              <div class="col-3">
                <div class="border-dashed rounded p-4">
                  <h1 class="fw-bolder mb-0" v-if="prospect_total.deviation">${{ formatNumber(prospect_total.deviation) }}</h1>
                  <h1 class="fw-bolder mb-0" v-else>$ 0</h1>
                  <p class="mb-0 fw-bold text-gray-500">Deviation</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row mt-10">
        <div class="tab-content" id="myTabContent">
          <div class="tab-pane fade show active" id="kt_tab_pane_1" role="tabpanel">
            <div class="row">
              <div class="col-4 mb-3" v-for="(data, prospect_customer_index) in prospect_customer"  :key="prospect_customer_index">
                <div class="card shadow-sm border-bottom border-5 border-primary" style="min-height: 420px">
                  <div class="row d-flex justify-content-between mt-5 ms-5 me-5">
                    <div class="col-2 p-1">
                      <span class="badge text-muted text-bg-light">{{ data.transaction_type.name }}</span>
                    </div>
                    <div class="col-3 p-1 d-flex justify-content-end">
                      <div v-if="data.transaction_type.name == 'PBTH'">
                        <span class=" fs-7 p-3 align-self-center badge rounded text-bg-light text-primary" v-if="data.sales.length > 0">Done</span>
                        <span class=" fs-7 p-3 align-self-center badge rounded text-bg-light text-primary" v-else>Pick Up</span>
                      </div>
                      <div v-else>
                        <span class=" fs-7 p-3 align-self-center badge rounded text-bg-light text-primary">Pick Up</span>
                      </div>
                    </div>
                  </div>
                  <div class="row mt-5 ms-5 me-5">
                    <div class="col p-1">
                      <h1 class="fw-bold">Airframe</h1>
                      <p class="text-muted fs-5 fw-bold" style="min-height: 50px">
                        Project for {{ data.registration }}
                      </p>
                      <p class="text-muted fs-8 fw-bold">
                        Remark for this project..
                      </p>
                    </div>
                  </div>
                  <div class="row mt-5 ms-5 me-5">
                    <div class="col-6 p-2" v-if="data.market_share ">
                      <div class="border-dashed p-2 rounded">
                        <p class="fw-bolder mb-0">${{ formatNumber(data.market_share) }}</p>
                        <p class="mb-0 fs-7 text-gray-500">
                          Total Market Share
                        </p>
                      </div>
                    </div>
                    <div class="col-6 p-2" v-else>
                      <div class="border-dashed p-2 rounded">
                        <p class="fw-bolder mb-0">$ 0</p>
                        <p class="mb-0 fs-7 text-gray-500">
                          Total Market Share
                        </p>
                      </div>
                    </div>
                    <div class="col-6 p-2" v-if="data.sales_plan">
                      <div class="border-dashed p-2 rounded">
                        <p class="fw-bolder mb-0">${{ formatNumber(data.sales_plan) }}</p>
                        <p class="mb-0 fs-7 text-gray-500">Total Salesplan</p>
                      </div>
                    </div>
                    <div class="col-6 p-2" v-else>
                      <div class="border-dashed p-2 rounded">
                        <p class="fw-bolder mb-0">$ 0</p>
                        <p class="mb-0 fs-7 text-gray-500">Total Salesplan</p>
                      </div>
                    </div>
                  </div>
                  <div class=" row mt-10 ms-5 me-5 mb-5 d-flex-justify-content-between">
                    <div v-if="data.transaction_type_id == 1 || data.transaction_type_id == 2" class="col-md-12 d-flex justify-content-end mt-10">
                      <div v-if="role == 'Administrator' || role == 'AMS' || role == 'AM'">
                        <nuxt-link :to="{ path: 'view-prospect-tmb', query: { id: data.id }}" class="btn btn-primary btn-sm">Pick Up</nuxt-link>
                      </div>
                      <div v-else>
                      <button type="button" class="btn btn-sm btn-primary" data-bs-toggle="modal" data-bs-target="#notAuthorized">
                        Pick Up
                      </button>
                      </div>
                    </div>

                    <div v-else class="col-md-12 d-flex justify-content-end mt-10">
                      <div v-if="role == 'Administrator' || role == 'AMS' || role == 'AM'">
                        <div v-if="data.sales.length > 0">
                          <nuxt-link :to="{ path: 'view-prospect-pbth', query: { id: data.id }}" class="btn btn-primary disabled btn-sm">
                            Done
                          </nuxt-link>
                        </div>
                        <div v-else>
                          <nuxt-link :to="{ path: 'view-prospect-pbth', query: { id: data.id }}" class="btn btn-primary btn-sm">Pick Up</nuxt-link>
                        </div>
                      </div>
                      <div v-else>
                        <nuxt-link :to="{ path: 'view-prospect-pbth', query: { id: data.id }}" class="btn btn-primary disabled btn-sm">
                          Done
                        </nuxt-link>
                      </div>
                    </div>
                    
                    <div class="modal fade" id="notAuthorized" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                      <div class="modal-dialog">
                        <div class="modal-content">
                          <div class="modal-header">
                            <h1 class="modal-title fs-5" id="exampleModalLabel">Notification</h1>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                          </div>
                          <div class="modal-body">
                            Sorry, You Are Not Allowed to Access This Feature
                          </div>
                          <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <tr v-if="prospect_customer.length < 1">
                  <div class="text-muted text-center fs-2">
                    Data not found
                  </div>
              </tr>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import debounce from 'lodash/debounce'
export default {
  layout: 'template',
  data() {
    return {
      role: this.$auth.user.role.name,
      prospect_customer: [],
      prospect_total: [],
      customer: {},
      customer_area: [],
      customer_region: [],
      customer_country: [],
      customer_image: [],
      customer_name: null,
      initial: null,
      }
    },
    created() {
      this.listProspectCustomer()
      this.Customer()
      this.checkRole()
  },
  methods: {
    authMessage() {
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
      toastr.error('Sorry, You Are Not Allowed to Access Prospect Page!')
    },
    checkRole(){
      if(this.role == 'TPC' || this.role == 'AMS' || this.role == 'AM' || this.role == 'Administrator'){
      } else {
        this.$router.push('/');
        this.authMessage()
      }
    },
    formatNumber(value) {
      let val = (value/1).toFixed(2).replace(',', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    },
    listProspectCustomer() {
      this.loading()
      this.$axios
        .get(`api/prospect-show/${this.$route.query.id}`)
        .then((response) => {
          // Data Prospect
          this.prospect_customer = response.data.data.prospect
          // Data Total
          this.prospect_total = response.data.data
          this.salesPlan = response.data.data.salesPlan
        }).catch((error) => {
        if (error.response.status == 404) {
          toastr.error(error.response.data.message)
          this.$router.push({
            name: 'my-prospect'
          });
        }
      })
    },
    Customer() {
      this.loading()
      this.$axios
        .get(`api/customer-show/${this.$route.query.id}`)
        .then((response) => {
          // Data Customer
          this.customer = response.data.data
          // Data Area Customer
          this.customer_area = response.data.data.ams_customers[0].area
          // Data Country Customer
          this.customer_country = response.data.data.country
          // Data Region Customer
          this.customer_region = response.data.data.country.region
          this.customer_image = response.data.data.full_path
          this.initial = this.customer.name.substring(0,1)
        })
        .catch((error) => {
        if (error.response.status == 404) {
          toastr.error(error.response.data.message)
          this.$router.push({
            name: 'my-prospect'
          });
        }
      })
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
    }
  }
</script>
<style>
</style>