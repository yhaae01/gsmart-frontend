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
            Customer Group
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
          <li class="breadcrumb-item text-muted">Customer Group</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of Customer Group</h3>
          <div class="card-toolbar">
            <button
              type="button"
              class="btn btn-sm btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#modal"
              @click="add()"
            >
              Add Customer Group
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
                name="search"
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

                    <th
                      v-if="order == 'code' && by == 'asc'"
                      @click="sort('code', 'desc')"
                      class="text-center"
                    >
                      Code
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'code' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Code
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th v-else @click="sort('code', 'asc')" class="text-center">
                      Code <i class="fa-solid fa-sort"></i>
                    </th>

                    <th
                      v-if="order == 'name' && by == 'asc'"
                      @click="sort('name', 'desc')"
                      class="text-center"
                    >
                      Name
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'name' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Name
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th v-else @click="sort('name', 'asc')" class="text-center">
                      Name <i class="fa-solid fa-sort"></i>
                    </th>

                    <th class="text-center">Country</th>

                    <th
                      v-if="order == 'status' && by == 'asc'"
                      @click="sort('status', 'desc')"
                      class="text-center"
                    >
                      Status
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'status' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Status
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('status', 'asc')"
                      class="text-center"
                    >
                      Status <i class="fa-solid fa-sort"></i>
                    </th>

                    <th class="text-center">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(
                      p_customer_group, p_customer_group_index
                    ) in customer_group.data"
                    :key="p_customer_group_index"
                  >
                    <td class="text-center">
                      {{ customer_group.from + p_customer_group_index }}.
                    </td>
                    <td class="text-center">{{ p_customer_group.code }}</td>
                    <td class="text-center">{{ p_customer_group.name }}</td>
                    <td class="text-center">
                      {{ p_customer_group.country.name }}
                    </td>
                    <td class="text-center">{{ p_customer_group.status }}</td>
                    <td class="d-flex justify-content-center">
                      <button
                        class="btn btn-sm btn-light me-5"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(p_customer_group)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
                      <button
                        class="btn btn-sm btn-light"
                        v-on:click="deleteData(p_customer_group.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="customer_group.data.length < 1">
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
                    v-for="(link, link_index) in customer_group.links"
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
            <h3 v-if="modal_create" class="modal-title">Add Customer Group</h3>
            <h3 v-else class="modal-title">Edit Customer Group</h3>

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
              <div class="row">
                <div class="col-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Customer Name</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="p_customer_group.name"
                      :class="{
                        'is-invalid': errors.name,
                      }"
                    />
                    <span v-if="errors.name" class="error invalid-feedback">{{
                      errors.name[0]
                    }}</span>
                  </div>
                </div>
                <div class="col-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Customer Code</label>
                    <input
                      type="text"
                      v-model="p_customer_group.code"
                      :class="{ 'is-invalid': errors.code }"
                      class="form-control"
                    />
                    <span v-if="errors.code" class="error invalid-feedback">{{
                      errors.code[0]
                    }}</span>
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Country</label>
                    <select
                      v-model="p_customer_group.country_id"
                      class="form-select"
                      :class="{ 'is-invalid': errors.country_id }"
                    >
                      <option :value="null" disabled>Select Country</option>
                      <option
                        v-for="country_options in country_option"
                        :value="country_options.id"
                        :class="{
                          'is-invalid': errors.country_id,
                        }"
                      >
                        {{ country_options.label }}
                      </option>
                    </select>
                    <span
                      v-if="errors.country_id"
                      class="error invalid-feedback"
                      >{{ errors.country_id[0] }}</span
                    >
                  </div>
                </div>
                <div class="col-6">
                  <div class="form-group mb-4">
                    <label class="form-label fw-bold">Group Type</label>
                    <select
                      v-model="p_customer_group.group_type"
                      class="form-select"
                      :class="{ 'is-invalid': errors.group_type }"
                    >
                      <option :value="null" disabled>Select Group Type</option>
                      <option :value="0">GA</option>
                      <option :value="1">NGA</option>
                    </select>
                    <span
                      v-if="errors.group_type"
                      class="error invalid-feedback"
                      >{{ errors.group_type[0] }}</span
                    >
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-6">
                  <div class="form-group mb-4">
                    <label class="form-label fw-bold">Status</label>
                    <select
                      v-model="p_customer_group.is_active"
                      class="form-select"
                      :class="{ 'is-invalid': errors.is_active }"
                    >
                      <option :value="null" disabled>Select Status</option>
                      <option :value="1">Active</option>
                      <option :value="0">Inactive</option>
                    </select>
                    <span
                      v-if="errors.is_active"
                      class="error invalid-feedback"
                      >{{ errors.is_active[0] }}</span
                    >
                  </div>
                </div>
              </div>

              <hr />

              <div id="kt_docs_repeater_basic">
                <h3 class="mb-0 mt-5">Customer</h3>
                <span
                  v-if="errors.customer_groups"
                  class="error invalid-feedback mb-5"
                >
                  {{ 'The customer field is required.' }}
                </span>
                <!-- Tombol untuk menambah area customer -->
                <div class="form-group mt-5">
                  <a
                    type="button"
                    @click="addCustomers()"
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
                      v-for="(data, index) in customer_groups"
                      :key="index"
                    >
                      <div class="form-group row mb-5">
                        <!-- Nomor urut area customer -->
                        <div
                          class="col px-1 d-flex justify-content-center align-items-end"
                        >
                          <h3 class="mb-4">{{ index + 1 }}.</h3>
                        </div>
                        <!-- Dropdown untuk memilih customer -->
                        <div class="col-md-10 text-center">
                          <label class="form-label">Customer</label>
                          <select
                            v-model="data.id"
                            class="form-select"
                            :class="{ 'is-invalid': errors.id }"
                          >
                            <option :value="null">Select Customer</option>
                            <option
                              v-for="customers in customer_options"
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
                            v-if="fail[index].customer_id"
                            class="error invalid-feedback"
                            >{{ fail[index].customer_id }}</span
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
  props: ['customers'],
  data() {
    return {
      role: this.$auth.user.role.name,
      customer_group: {
        data: [],
        link: [],
      },
      users: [],
      option_am: [],
      fail: [],

      area_options: [],
      customer_group_options: [],
      country_option: [],

      customer_id: null,

      customer_groups: [],
      p_customer_group: {
        id: null,
        name: null,
        code: null,
        country_id: [],
        group_type: null,
        is_active: null,
        customers: [],
      },
      modal_create: false,
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,
      errors: {
        name: null,
        code: null,
        country_id: null,

        customer_id: null,
        customer_groups: null,
      },
    }
  },
  created() {
    this.list()
    this.listCustomer()
    this.listCountry()
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
      paginate = paginate || `/api/customer-group`
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
          this.customer_group = response.data.data
          this.current_page = this.customer_group.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    listCustomer() {
      this.$axios
        .get('api/customer')
        .then((response) => {
          this.customer_options = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    listCountry() {
      this.$axios
        .get('api/countries', {
          params: {
            paginate: 1000,
          },
        })
        .then((response) => {
          this.country_option = response.data.data.data
        })
        .catch((error) => console.log(error))
    },

    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.customers.last_page) {
        this.current_page = this.customers.last_page
      }
      let url = new URL(this.customers.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.list(new_url)
    }, 500),
    addCustomers() {
      this.customer_groups.push({
        id: null,
      })
      this.fail.push({
        id: null,
      })
    },
    remove(index) {
      this.customer_groups.splice(index, 1)
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
      this.customer_groups.push({
        id: null,
      })
      this.fail.push({
        id: null,
      })
      this.listCustomer()
    },

    create() {
      this.loading()
      this.$axios
        .post('/api/customer-group-create', {
          name: this.p_customer_group.name,
          code: this.p_customer_group.code,
          country_id: this.p_customer_group.country_id,
          group_type: this.p_customer_group.group_type,
          is_active: this.p_customer_group.is_active,

          customer_group: this.customer_groups,
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
      this.$axios.get('/api/customer-group/' + item.id).then((result) => {
        this.modal_create = false
        this.p_customer_group.id = result.data.data.id
        this.p_customer_group.code = result.data.data.code
        this.p_customer_group.name = result.data.data.name
        this.p_customer_group.country_id = result.data.data.country_id
        this.p_customer_group.is_active = result.data.data.is_active
        this.p_customer_group.group_type = result.data.data.group_type

        this.customer_groups = result.data.data.customers
        for (let i = 0; i < this.customer_groups.length; i++) {
          this.fail.push({
            id: null,
          })
        }
        this.isDisabled = true
        console.log(this.fail)
      })
    },

    update() {
      this.loading()
      this.$axios
        .put('/api/customer-group-update/' + this.p_customer_group.id, {
          name: this.p_customer_group.name,
          code: this.p_customer_group.code,
          country_id: this.p_customer_group.country_id,
          group_type: this.p_customer_group.group_type,
          is_active: this.p_customer_group.is_active,

          customer_group: this.customer_groups,
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
      this.customer_groups = []
      this.p_customer_group.id = null
      this.p_customer_group.name = null
      this.p_customer_group.code = null
      this.p_customer_group.country_id = null
      this.p_customer_group.customer_id = null

      this.errors.name = null
      this.errors.code = null
      this.errors.country_id = null
      this.errors.customer_id = null

      this.error = []
      this.fail = []
    },
    closeModal() {
      document.getElementById('close_modal').click()
      this.clearForm()
    },
  },
}
</script>
