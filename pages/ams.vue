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
          class="page-title d-flex flex-column justify-content-center flex-wrap me-3"
        >
          <!--begin::Title-->
          <p
            class="page-heading d-flex text-dark fs-6 flex-column justify-content-center my-0 fw-bold"
          >
            AMS
          </p>
          <!--end::Title-->
        </div>
        <ul
          class="breadcrumb breadcrumb-separatorless fw-semibold fs-7 my-0 pt-1"
        >
          <!--begin::Item-->
          <li class="breadcrumb-item text-muted">
            <nuxt-link to="/" class="text-muted text-hover-primary"
              >Dashboard</nuxt-link
            >
          </li>
          <li class="breadcrumb-item">
            <span class="bullet bg-gray-400 w-5px h-2px"></span>
          </li>
          <li class="breadcrumb-item text-muted">AMS</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of AMS</h3>
          <div class="card-toolbar">
            <button
              type="button"
              class="btn btn-sm btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#modal"
              @click="add()"
            >
              Add AMS
            </button>
          </div>
        </div>
        <div class="card-body">
          <div class="row d-flex align-items-center">
            <!--begin::Input group-->
            <div class="position-relative me-md-2">
              <!--begin::Svg Icon | path: icons/duotune/general/gen021.svg-->
              <span
                class="svg-icon svg-icon-3 svg-icon-gray-500 position-absolute top-50 translate-middle ms-6"
              >
                <svg
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect
                    opacity="0.5"
                    x="17.0365"
                    y="15.1223"
                    width="8.15546"
                    height="2"
                    rx="1"
                    transform="rotate(45 17.0365 15.1223)"
                    fill="currentColor"
                  ></rect>
                  <path
                    d="M11 19C6.55556 19 3 15.4444 3 11C3 6.55556 6.55556 3 11 3C15.4444 3 19 6.55556 19 11C19 15.4444 15.4444 19 11 19ZM11 5C7.53333 5 5 7.53333 5 11C5 14.4667 7.53333 17 11 17C14.4667 17 17 14.4667 17 11C17 7.53333 14.4667 5 11 5Z"
                    fill="currentColor"
                  ></path>
                </svg>
              </span>
              <!--end::Svg Icon-->
              <input
                type="text"
                class="form-control form-control-solid ps-10"
                initial="search"
                v-model="search"
                placeholder="Search"
              />
            </div>
            <!--end::Input group-->
          </div>
          <div class="py-5">
            <div class="table-responsive">
              <table class="table table-row-bordered table-row-gray-200 gy-4">
                <thead>
                  <tr class="fw-bold fs-6 text-gray-800">
                    <th class="text-center">No</th>
                    <!-- Start Initial sorting -->
                    <th
                      v-if="order == 'initial' && by == 'asc'"
                      @click="sort('initial', 'desc')"
                      class="text-center"
                    >
                      Initial
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'initial' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Initial
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('initial', 'asc')"
                      class="text-center"
                    >
                      Initial <i class="fa-solid fa-sort"></i>
                    </th>
                    <!-- End Initial sorting -->

                    <!-- Start user sorting -->
                    <th
                      v-if="order == 'user' && by == 'asc'"
                      @click="sort('user', 'desc')"
                      class="text-center"
                    >
                      AMS
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'user' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      AMS
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th v-else @click="sort('user', 'asc')" class="text-center">
                      AMS <i class="fa-solid fa-sort"></i>
                    </th>
                    <!-- End user sorting -->

                    <!-- Start AM sorting -->
                    <th
                      v-if="order == 'am' && by == 'asc'"
                      @click="sort('am', 'desc')"
                      class="text-center"
                    >
                      AM
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'am' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      AM
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th v-else @click="sort('am', 'asc')" class="text-center">
                      AM <i class="fa-solid fa-sort"></i>
                    </th>
                    <!-- End AM sorting -->

                    <!-- <th class="text-center">Area</th> -->
                    <!-- <th class="text-center">Customer</th> -->
                    <th class="text-center">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(p_ams, p_ams_index) in ams.data"
                    :key="p_ams_index"
                  >
                    <td class="text-center">{{ ams.from + p_ams_index }}.</td>
                    <td class="text-center">{{ p_ams.initial }}</td>
                    <td class="text-center">{{ p_ams.user.name }}</td>
                    <td class="text-center">
                      <span v-if="p_ams.am_initial">{{
                        p_ams.am_initial
                      }}</span>
                      <span v-else>-</span>
                    </td>

                    <!-- List Area -->
                    <!-- <td class="text-center">
                      <span
                        v-for="(
                          customer, customer_index
                        ) in p_ams.ams_customers"
                        :key="customer_index"
                      >
                        {{ customer.area.name }}
                        <br />
                      </span>
                    </td> -->

                    <!-- List Customers -->
                    <!-- <td class="text-center">
                      <span
                        v-for="(
                          customer, customer_index
                        ) in p_ams.ams_customers"
                        :key="customer_index"
                      >
                        {{ customer.customer.name }}
                        <br />
                      </span>
                    </td> -->
                    <td class="d-flex justify-content-center">
                      <button
                        class="btn btn-sm btn-light me-5"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(p_ams)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
                      <button
                        class="btn btn-sm btn-light"
                        v-on:click="deleteData(p_ams.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="ams.data.length < 1">
                    <td colspan="8">
                      <div class="text-muted text-center">Data not found</div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
        <div class="card-footer">
          <div class="row">
            <div class="col d-flex justify-content-start align-items-center">
              <nav aria-label="Page navigation example">
                <ul class="pagination">
                  <li class="page-item align-self-center">Rows per page:</li>
                  <li class="page-item">
                    <select
                      class="form-control form-control-sm"
                      v-model="paginate"
                      @change="list()"
                    >
                      <option value="10">10</option>
                      <option value="25">25</option>
                      <option value="50">50</option>
                      <option value="100">100</option>
                    </select>
                  </li>
                </ul>
              </nav>
            </div>
            <div
              class="col col-lg-8 d-flex justify-content-end align-items-center"
            >
              <nav>
                <ul class="pagination">
                  <!-- Start pagination -->
                  <li
                    v-for="(link, link_index) in ams.links"
                    :key="link_index"
                    class="page-item"
                    :class="{ disabled: !link.url, active: link.active }"
                  >
                    <a
                      href="javascript:void(0)"
                      @click="list(link.url)"
                      class="page-link"
                    >
                      <span v-if="link.label == '&laquo; Previous'">
                        <i class="fa-solid fa-caret-left"></i>
                      </span>
                      <span v-else-if="link.label == 'Next &raquo;'">
                        <i class="fa-solid fa-caret-right"></i>
                      </span>
                      <span v-else>
                        {{ link.label }}
                      </span>
                    </a>
                  </li>
                  <!-- End pagination -->
                </ul>
              </nav>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="modal fade" tabindex="-1" id="modal" data-bs-backdrop="static">
      <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h3 v-if="modal_create" class="modal-title">Add AMS</h3>
            <h3 v-else class="modal-title">Edit AMS</h3>

            <!--begin::Close-->
            <div
              class="btn btn-icon btn-sm btn-active-light-primary ms-2"
              data-bs-dismiss="modal"
              aria-label="Close"
            >
              <span class="svg-icon svg-icon-1" @click="closeModal()">
                <svg
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect
                    opacity="0.5"
                    x="6"
                    y="17.3137"
                    width="16"
                    height="2"
                    rx="1"
                    transform="rotate(-45 6 17.3137)"
                    fill="currentColor"
                  ></rect>
                  <rect
                    x="7.41422"
                    y="6"
                    width="16"
                    height="2"
                    rx="1"
                    transform="rotate(45 7.41422 6)"
                    fill="currentColor"
                  ></rect>
                </svg>
              </span>
            </div>
            <!--end::Close-->
          </div>
          <div class="modal-body">
            <form @submit.prevent="submit">
              <div class="form-group mb-3">
                <label class="form-label fw-bold">Initial</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="p_ams.initial"
                  :class="{
                    'is-invalid': errors.initial,
                  }"
                />
                <span v-if="errors.initial" class="error invalid-feedback">{{
                  errors.initial[0]
                }}</span>
              </div>
              <div class="form-group mb-3">
                <label class="form-label fw-bold">AMS</label>
                <select
                  v-model="p_ams.user_id"
                  class="form-select"
                  :class="{ 'is-invalid': errors.user_id }"
                >
                  <option
                    v-for="user_options in users"
                    :value="user_options.id"
                  >
                    {{ user_options.name }} - {{ user_options.email }}
                  </option>
                </select>
                <span v-if="errors.user_id" class="error invalid-feedback">{{
                  errors.user_id[0]
                }}</span>
              </div>
              <div class="form-group mb-3">
                <label class="form-label fw-bold">AM</label>
                <select
                  v-model="p_ams.am_id"
                  class="form-select"
                  :class="{ 'is-invalid': errors.am_id }"
                >
                  <option v-for="am_option in option_am" :value="am_option.id">
                    {{ am_option.user.name }} - {{ am_option.user.email }}
                  </option>
                </select>
                <span v-if="errors.am_id" class="error invalid-feedback">{{
                  errors.am_id[0]
                }}</span>
              </div>

              <hr />

              <div id="kt_docs_repeater_basic">
                <h3 class="mb-0 mt-5">Area Customer</h3>
                <span
                  v-if="errors.area_customers"
                  class="error invalid-feedback mb-5"
                >
                  {{ 'The customer field is required.' }}
                </span>
                <!-- Tombol untuk menambah area customer -->
                <div class="form-group mt-5">
                  <a
                    type="button"
                    @click="addAreaCustomer()"
                    class="btn btn-primary btn-sm"
                  >
                    Add
                  </a>
                </div>
                <!-- Form untuk menampilkan area customer -->
                <div class="form-group">
                  <div data-repeater-list="kt_docs_repeater_basic">
                    <div
                      data-repeater-item
                      v-for="(data, index) in area_customers"
                      :key="index"
                    >
                      <div class="form-group row mb-5">
                        <!-- Nomor urut area customer -->
                        <div
                          class="col px-1 d-flex justify-content-center align-items-end"
                        >
                          <h3 class="mb-4">{{ index + 1 }}.</h3>
                        </div>
                        <!-- Dropdown untuk memilih area -->
                        <div class="col-md-5 text-center">
                          <label class="form-label">Area</label>
                          <select
                            v-model="data.area_id"
                            class="form-select"
                            :class="{ 'is-invalid': errors.area_id }"
                          >
                            <option :value="null">Select Area</option>
                            <option
                              v-for="areas in area_options"
                              :value="areas.id"
                            >
                              {{ areas.name }}
                            </option>
                          </select>
                          <span
                            v-if="errors.area_id"
                            class="error invalid-feedback"
                            >{{ errors.area_id[0] }}</span
                          >
                        </div>
                        <!-- Dropdown untuk memilih customer -->
                        <div class="col-md-5 text-center">
                          <label class="form-label">Customer</label>
                          <select
                            v-model="data.customer_id"
                            class="form-select"
                            :class="{ 'is-invalid': errors.customer_id }"
                          >
                            <option :value="null">Select Customer</option>
                            <option
                              v-for="customers in customer_group_options"
                              :value="customers.id"
                            >
                              {{ customers.label }}
                            </option>
                          </select>
                          <span
                            v-if="errors.customer_id"
                            class="error invalid-feedback"
                            >{{ errors.customer_id[0] }}</span
                          >
                          <!-- Pesan error jika ada -->
                          <span
                            v-if="fail[index].ams"
                            class="error invalid-feedback"
                            >{{ fail[index].ams }}</span
                          >
                        </div>
                        <!-- Tombol untuk menghapus area customer -->
                        <div class="col">
                          <a
                            type="button"
                            @click="remove(index)"
                            class="btn btn-light-danger mt-3 mt-md-8"
                          >
                            <i class="la la-trash-o"></i>
                          </a>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="row mt-10">
                <div class="col">
                  <button
                    type="button"
                    class="btn btn-light"
                    data-bs-dismiss="modal"
                    id="close_modal"
                    @click="closeModal()"
                  >
                    Back
                  </button>
                </div>
                <div class="col d-flex justify-content-end">
                  <button type="submit" class="btn btn-primary">Save</button>
                </div>
              </div>
            </form>
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
  props: ['ams_customers'],
  data() {
    return {
      role: this.$auth.user.role.name,
      ams: {
        data: [],
        link: [],
      },
      users: [],
      option_am: [],
      fail: [],

      area_options: [],
      customer_group_options: [],

      area_id: null,
      customer_id: null,

      area_customers: [],
      p_ams: {
        id: null,
        initial: null,
        user_id: null,
        am_id: null,
        am_initial: null,
        ams_customers: [],
      },
      modal_create: false,
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,
      errors: {
        initial: null,
        user_id: null,
        am_id: null,
        area_id: null,
        customer_id: null,
        area_customers: null,
      },
    }
  },
  created() {
    this.list()
    this.listAM()
    this.user()
    this.listArea()
    this.listCustomerGroup()
  },
  watch: {
    search: debounce(function () {
      this.list()
    }, 500),
  },
  mounted() {
    this.redirectToDashboard()
  },
  methods: {
    redirectToDashboard() {
      if (this.role !== 'Administrator') {
        this.$router.push('/')
      }
    },
    sort(order, by) {
      this.order = order
      this.by = by
      this.list()
    },
    list(paginate) {
      this.loading()
      paginate = paginate || `/api/ams`
      this.$axios
        .get(paginate, {
          params: {
            search: this.search,
            order: this.order,
            by: this.by,
            paginate: this.paginate,
          },
        })
        .then((response) => {
          this.ams = response.data.data
          this.current_page = this.ams.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    listArea() {
      this.$axios
        .get('api/area')
        .then((response) => {
          this.area_options = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    listAM() {
      this.$axios
        .get('/api/am')
        .then((response) => {
          this.option_am = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    listCustomerGroup() {
      this.$axios
        .get('api/customer-group')
        .then((response) => {
          this.customer_group_options = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.ams.last_page) {
        this.current_page = this.ams.last_page
      }
      let url = new URL(this.ams.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.list(new_url)
    }, 500),
    addAreaCustomer() {
      this.area_customers.push({
        area_id: null,
        customer_id: null,
      })
      this.fail.push({
        area_id: null,
        customer_id: null,
      })
    },
    remove(index) {
      this.area_customers.splice(index, 1)
    },
    submit() {
      if (this.modal_create) {
        this.create()
      } else {
        this.update()
      }
    },
    add() {
      this.clearForm()
      this.modal_create = true
      this.area_customers.push({
        area_id: null,
        customer_id: null,
      })
      this.fail.push({
        area_id: null,
        customer_id: null,
      })
      this.listArea()
      this.listCustomerGroup()
    },

    create() {
      this.loading()
      this.$axios
        .post('/api/ams-create', {
          initial: this.p_ams.initial,
          user_id: this.p_ams.user_id,
          am_id: this.p_ams.am_id,

          area_customer: this.area_customers,
        })
        .then((response) => {
          toastr.success(response.data.message)
          this.list()
          this.closeModal()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors

            toastr.error(error.response.data.message)
          }
        })
    },

    // Show data when edit button click
    edit(item) {
      this.clearForm()
      this.$axios.get('/api/ams-show/' + item.id).then((result) => {
        this.modal_create = false
        this.p_ams.id = result.data.data.id
        this.p_ams.user_id = result.data.data.user_id
        this.p_ams.initial = result.data.data.initial
        this.p_ams.am_id = result.data.data.am_id

        this.area_customers = result.data.data.ams_customers
        for (let i = 0; i < this.area_customers.length; i++) {
          this.fail.push({
            area_id: null,
            customer_id: null,
          })
        }
        this.isDisabled = true
        console.log(this.fail)
      })
    },

    update() {
      this.loading()
      this.$axios
        .put('/api/ams-update/' + this.p_ams.id, {
          initial: this.p_ams.initial,
          user_id: this.p_ams.user_id,
          am_id: this.p_ams.am_id,

          area_customer: this.area_customers,
        })
        .then((response) => {
          toastr.success(response.data.message)
          this.list()
          this.closeModal()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          }
        })
    },

    deleteData(id) {
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
      })
        .then((result) => {
          if (result.isConfirmed) {
            this.$axios.delete('/api/ams-delete/' + id).then((response) => {
              toastr.success(response.data.message)
              this.list()
            })
          }
        })
        .catch((error) => {
          console.log(error)
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
    clearForm() {
      this.area_customers = []
      this.p_ams.id = null
      this.p_ams.initial = null
      this.p_ams.user_id = null
      this.p_ams.am_id = null
      this.p_ams.area_id = null
      this.p_ams.customer_id = null

      this.errors.initial = null
      this.errors.user_id = null
      this.errors.am_id = null
      this.errors.customer_id = null
      this.errors.area_id = null

      this.error = []
      this.fail = []
    },
    closeModal() {
      document.getElementById('close_modal').click()
      this.clearForm()
    },
    user() {
      this.$axios
        .get('/api/users')
        .then((response) => {
          this.users = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
  },
}
</script>
