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
            class="page-heading d-flex text-dark fs-6 flex-column justify-content-center my-0"
          >
            Customers
          </p>
          <!--end::Title-->
        </div>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mt-5 mb-20">
      <div class="card shadow-sm">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of Customers</h3>
          <div class="card-toolbar">
            <!-- <button
              type="button"
              class="btn btn-sm btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#modal"
              @click="add()"
            >
              Add Customer
            </button> -->
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

                    <th
                      v-if="order == 'group' && by == 'asc'"
                      @click="sort('group', 'desc')"
                      class="text-center"
                    >
                      Group Type
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'group' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Group Type
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('group', 'asc')"
                      class="text-center"
                    >
                      Group Type <i class="fa-solid fa-sort"></i>
                    </th>

                    <th
                      v-if="order == 'country' && by == 'asc'"
                      @click="sort('country', 'desc')"
                      class="text-center"
                    >
                      Country
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'country' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Country
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('country', 'asc')"
                      class="text-center"
                    >
                      Country <i class="fa-solid fa-sort"></i>
                    </th>

                    <th
                      v-if="order == 'region' && by == 'asc'"
                      @click="sort('region', 'desc')"
                      class="text-center"
                    >
                      Region
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'region' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Region
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('region', 'asc')"
                      class="text-center"
                    >
                      Region <i class="fa-solid fa-sort"></i>
                    </th>

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
                    v-for="(customer, customer_index) in customer.data"
                    :key="customer_index"
                  >
                    <td class="text-center">
                      <span v-if="customer_page.from == 1">
                        {{ customer_page.from + customer_index }}
                      </span>
                      <span v-else>
                        {{ parseInt(customer_index) + 1 }}
                      </span>
                    </td>
                    <td class="text-center">{{ customer.code }}</td>
                    <td class="text-center">{{ customer.name }}</td>
                    <td class="text-center">{{ customer.group }}</td>
                    <td class="text-center">{{ customer.country.name }}</td>
                    <td class="text-center">
                      {{ customer.country.region.name }}
                    </td>
                    <td class="text-center">{{ customer.status }}</td>
                    <td class="d-flex justify-content-center">
                      <button
                        class="btn btn-sm btn-light mx-2"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(customer)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
                      <button
                        class="btn btn-sm btn-light"
                        @click="hapus(customer.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="customers.length < 1">
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
            <div
              class="col col-md-4 d-flex justify-content-start align-items-center"
            >
              <nav aria-label="Page navigation example">
                <ul class="pagination">
                  <li class="page-item align-self-center">Rows per page:</li>
                  <li class="page-item">
                    <select
                      class="form-control form-control-sm"
                      v-model="paginate"
                      @change="listCustomer()"
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
              class="col col-md-8 d-flex justify-content-end align-items-center"
            >
              <nav>
                <ul class="pagination">
                  <!-- Start pagination -->
                  <li
                    v-for="(link, link_index) in customer.links"
                    :key="link_index"
                    class="page-item"
                    :class="{ disabled: !link.url, active: link.active }"
                  >
                    <a
                      href="javascript:void(0)"
                      @click="listCustomer(link.url)"
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
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h3 v-if="modal_create" class="modal-title">Add Customer</h3>
            <h3 v-else class="modal-title">Edit Customer</h3>

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
                  <div class="form-group mb-4">
                    <label class="form-label fw-bold">Customer Name</label>
                    <input
                      v-if="modal_create == true"
                      type="text"
                      v-model="customer.name"
                      :class="{ 'is-invalid': errors.name }"
                      class="form-control"
                    />
                    <input
                      v-if="modal_create == false"
                      disabled
                      type="text"
                      v-model="customer.name"
                      :class="{ 'is-invalid': errors.name }"
                      class="form-control"
                    />
                    <span v-if="errors.name" class="error invalid-feedback">{{
                      errors.name[0]
                    }}</span>
                  </div>
                  <div class="form-group mb-4">
                    <label class="form-label fw-bold">Region</label>
                    <multiselect
                      v-if="modal_create == true"
                      v-model="region_value"
                      :options="region"
                      open-direction="bottom"
                      placeholder=""
                      label="name"
                      :searchable="true"
                      :class="{ 'is-invalid': errors.region_id }"
                    ></multiselect>
                    <input
                      v-if="modal_create == false"
                      disabled
                      type="text"
                      v-model="customer.region"
                      class="form-control"
                    />
                    <span
                      v-if="errors.region_id"
                      class="error invalid-feedback"
                      >{{ errors.region_id[0] }}</span
                    >
                  </div>
                </div>
                <div class="col-6">
                  <div class="form-group mb-4">
                    <label class="form-label fw-bold">Customer Code</label>
                    <input
                      v-if="modal_create == true"
                      type="text"
                      v-model="customer.code"
                      :class="{ 'is-invalid': errors.code }"
                      class="form-control"
                    />
                    <input
                      v-if="modal_create == false"
                      disabled
                      type="text"
                      v-model="customer.customer_code"
                      class="form-control"
                    />
                    <span v-if="errors.code" class="error invalid-feedback">{{
                      errors.code[0]
                    }}</span>
                  </div>
                  <div class="form-group mb-4">
                    <label class="form-label fw-bold">Country</label>
                    <multiselect
                      v-if="modal_create == true"
                      v-model="country_value"
                      :options="country"
                      open-direction="bottom"
                      :disabled="isDisabled"
                      :searchable="true"
                      placeholder=""
                      label="name"
                      :class="{ 'is-invalid': errors.country_id }"
                    ></multiselect>
                    <input
                      v-if="modal_create == false"
                      disabled
                      type="text"
                      v-model="customer.country"
                      class="form-control"
                    />
                    <span
                      v-if="errors.country_id"
                      class="error invalid-feedback"
                      >{{ errors.country_id[0] }}</span
                    >
                  </div>
                </div>
                <div class="col-6">
                  <label class="form-label fw-bold">Status</label>
                  <select
                    v-model="customer.is_active"
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
                <div class="col-6">
                  <label class="form-label fw-bold">Group Type</label>
                  <select
                    v-if="modal_create == true"
                    v-model="customer.group_type"
                    class="form-select"
                    :class="{ 'is-invalid': errors.group_type }"
                  >
                    <option value="null" disabled>Select Group Type</option>
                    <option :value="0">GA</option>
                    <option :value="1">NGA</option>
                  </select>
                  <select
                    v-if="modal_create == false"
                    disabled
                    v-model="customer.group_type"
                    class="form-select"
                    :class="{ 'is-invalid': errors.group_type }"
                  >
                    <option value="null" disabled>Select Group Type</option>
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
              <hr />
              <div id="kt_docs_repeater_basic">
                <h3 class="mb-0 mt-5">Area & AMS</h3>
                <span
                  v-if="errors.area_ams"
                  class="error invalid-feedback mb-5"
                  >{{ 'The area & ams field is required.' }}</span
                >
                <!--begin::Form group-->
                <div class="form-group mt-5">
                  <a
                    type="button"
                    @click="addAreaAms()"
                    class="btn btn-primary btn-sm"
                  >
                    Add
                  </a>
                </div>
                <!--end::Form group-->
                <div class="form-group">
                  <div data-repeater-list="kt_docs_repeater_basic">
                    <div
                      data-repeater-item
                      v-for="(data, index) in area_ams"
                      :key="index"
                    >
                      <div class="form-group row mb-5">
                        <div
                          class="col px-1 d-flex justify-content-center align-items-end"
                        >
                          <h3 class="mb-4">{{ index + 1 }}.</h3>
                        </div>
                        <div class="col-md-5 text-center">
                          <label class="form-label">Area</label>
                          <multiselect
                            v-model="data.area"
                            placeholder="Search and select Area"
                            label="name"
                            :searchable="true"
                            :options="area"
                            :class="{
                              'is-invalid': fail[index].area,
                            }"
                          ></multiselect>
                          <span class="error invalid-feedback">{{
                            fail[index].area
                          }}</span>
                        </div>
                        <div class="col-md-5 text-center">
                          <label class="form-label">AMS</label>
                          <multiselect
                            v-model="data.ams"
                            placeholder="Search and select AMS"
                            label="initial"
                            :searchable="true"
                            :options="ams"
                            :class="{
                              'is-invalid': fail[index].ams,
                            }"
                          ></multiselect>
                          <span
                            v-if="fail[index].ams"
                            class="error invalid-feedback"
                            >{{ fail[index].ams }}</span
                          >
                        </div>
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
                <!--end::Form group-->
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
  name: 'CustomerPage',
  layout: 'template',
  data() {
    return {
      role: this.$auth.user.role.name,
      customer_page: [],
      area_ams: [],
      fail: [],
      country_value: null,
      region_value: null,
      region: [],
      country: [],
      ams: [],
      area: [],
      modal_create: false,
      customer: {
        id: null,
        name: null,
        code: null,
        group_type: null,
        is_active: null,
        customer_code: {
          code: null,
        },
      },
      item: {
        name: null,
        code: null,
        country: {
          name: null,
          region: {
            name: null,
          },
        },
      },
      customers: {
        data: [],
        link: [],
      },
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,

      search_country: null,
      isDisabled: true,
      errors: [],
    }
  },
  created() {
    this.listCustomer()
    this.listRegion()
    this.listAMS()
    this.listArea()
  },
  mounted() {
    KTFormRepeaterBasic.init()
    this.redirectToDashboard()
  },
  watch: {
    search: debounce(function () {
      this.listCustomer()
    }, 500),
    region_value: debounce(function () {
      this.clear()
      this.listCountry()
    }, 100),
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
      this.listCustomer()
    },
    listCustomer(paginate) {
      this.loading()
      paginate = paginate || `/api/customer`
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
          this.customer = response.data.data
          this.customer_page = response.data.data
          this.current_page = this.customer.current_page
          Swal.close()
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
    addAreaAms() {
      this.area_ams.push({
        area: null,
        ams: null,
      })
      this.fail.push({
        area: null,
        ams: null,
      })
    },
    remove(index) {
      this.area_ams.splice(index, 1)
    },
    listRegion() {
      this.$axios
        .get('api/region', {
          params: {
            paginate: 1000,
          },
        })
        .then((response) => {
          this.region = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    listCountry() {
      if (this.region_value) {
        this.$axios
          .get(`/api/region`, {
            params: {
              search: this.region_value.name,
              paginate: 1000,
            },
          })
          .then((response) => {
            this.country = response.data.data.data[0].countries
          })
          .catch((error) => console.log(error))
      }
    },
    listArea() {
      this.$axios
        .get('api/area', {
          params: {
            paginate: 1000,
          },
        })
        .then((response) => {
          this.area = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    listAMS() {
      this.$axios
        .get('api/ams', {
          params: {
            paginate: 1000,
          },
        })
        .then((response) => {
          this.ams = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
    create() {
      this.loading()
      if (this.country_value == null || this.country_value == '') {
        this.$axios
          .post('api/customer-create', {
            name: this.customer.name,
            code: this.customer.code,
            group_type: this.customer.group_type,
            is_active: this.customer.is_active,
            country_id: this.country_value,
            region_id: this.region_value,
            area_ams: this.area_ams,
          })
          .then((response) => {
            toastr.success(response.data.message)
            this.listCustomer()
            this.closeModal()
          })
          .catch((error) => {
            if (error.response.status == 422) {
              this.errors = error.response.data.errors
              for (let i = 0; i < this.area_ams.length; i++) {
                if (this.area_ams[i].area == null) {
                  this.fail[i].area = 'The area field is required.'
                } else {
                  this.fail[i].area = null
                }
                if (this.area_ams[i].ams == null) {
                  this.fail[i].ams = 'The ams field is required.'
                } else {
                  this.fail[i].ams = null
                }
              }
              toastr.error(error.response.data.message)
            }
          })
      } else {
        this.$axios
          .post('api/customer-create', {
            name: this.customer.name,
            code: this.customer.code,
            group_type: this.customer.group_type,
            is_active: this.customer.is_active,
            country_id: this.country_value.id,
            region_id: this.region_value.id,
            area_ams: this.area_ams,
          })
          .then((response) => {
            toastr.success(response.data.message)
            this.listCustomer()
            this.closeModal()
          })
          .catch((error) => {
            if (error.response.status == 422) {
              this.errors = error.response.data.errors
              for (let i = 0; i < this.area_ams.length; i++) {
                if (this.area_ams[i].area == null) {
                  this.fail[i].area = 'The area field is required.'
                } else {
                  this.fail[i].area = null
                }
                if (this.area_ams[i].ams == null) {
                  this.fail[i].ams = 'The ams field is required.'
                } else {
                  this.fail[i].ams = null
                }
              }
              toastr.error(error.response.data.message)
            }
          })
      }
    },
    update() {
      this.loading()
      if (this.country_value == null || this.country_value == '') {
        this.$axios
          .put('/api/customer-update/' + this.customer.id, {
            name: this.customer.name,
            code: this.customer.code,
            group_type: this.customer.group_type,
            is_active: this.customer.is_active,
            country_id: this.country_value,
            region_id: this.region_value,
            area_ams: this.area_ams,
          })
          .then((response) => {
            toastr.success(response.data.message)
            this.listCustomer()
            this.closeModal()
          })
          .catch((error) => {
            if (error.response.status == 422) {
              this.errors = error.response.data.errors
              for (let i = 0; i < this.area_ams.length; i++) {
                if (this.area_ams[i].area == null) {
                  this.fail[i].area = 'The area field is required.'
                } else {
                  this.fail[i].area = null
                }
                if (this.area_ams[i].ams == null) {
                  this.fail[i].ams = 'The ams field is required.'
                } else {
                  this.fail[i].ams = null
                }
              }
              toastr.error(error.response.data.message)
            }
          })
      } else {
        this.$axios
          .put('/api/customer-update/' + this.customer.id, {
            name: this.customer.name,
            code: this.customer.code,
            group_type: this.customer.group_type,
            is_active: this.customer.is_active,
            country_id: this.country_value.id,
            region_id: this.region_value.id,
            area_ams: this.area_ams,
          })
          .then((response) => {
            toastr.success(response.data.message)
            this.listCustomer()
            this.closeModal()
          })
          .catch((error) => {
            if (error.response.status == 500) {
              // this.errors = error.response.data.errors
              for (let i = 0; i < this.area_ams.length; i++) {
                if (this.area_ams[i].area == null) {
                  this.fail[i].area = 'The area field is required.'
                } else {
                  this.fail[i].area = null
                }
                if (this.area_ams[i].ams == null) {
                  this.fail[i].ams = 'The ams field is required.'
                } else {
                  this.fail[i].ams = null
                }
              }
              toastr.error(error.response.data.message)
            }
          })
      }
    },
    edit(item) {
      this.clearForm()
      this.$axios.get('/api/customer-show/' + item.id).then((result) => {
        this.listRegion()
        this.listAMS()
        this.listArea()
        this.modal_create = false
        this.customer.id = result.data.data.id
        this.customer.name = result.data.data.name
        this.customer.code = result.data.data.code
        this.customer.customer_code = result.data.data.customer_code.code
        this.customer.group_type = result.data.data.group_type
        this.customer.is_active = result.data.data.is_active
        this.country_value = result.data.data.country
        this.customer.country = result.data.data.country.name
        this.customer.region = result.data.data.country.region.name
        this.region_value = result.data.data.country.region
        this.area_ams = result.data.data.ams_customers
        for (let i = 0; i < this.area_ams.length; i++) {
          this.fail.push({
            area: null,
            ams: null,
          })
        }
        this.isDisabled = true
        console.log(this.fail)
      })
    },
    hapus(id) {
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
            this.$axios
              .delete('/api/customer-delete/' + id)
              .then((response) => {
                toastr.success(response.data.message)
                this.listCustomer()
              })
          }
        })
        .catch((error) => {
          console.log(error)
        })
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
      this.area_ams.push({
        area: null,
        ams: null,
      })
      this.fail.push({
        area: null,
        ams: null,
      })
      this.listRegion()
      this.listAMS()
      this.listArea()
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
    closeModal() {
      document.getElementById('close_modal').click()
      this.clearForm()
    },
    clear() {
      if (this.region_value == null) {
        this.isDisabled = true
        this.country = []
        this.country_value = null
      } else {
        this.country = []
        this.isDisabled = false
      }
    },
    clearForm() {
      this.region = []
      this.region_value = null
      this.country = []
      this.country_value = null
      this.area_ams = []
      this.fail = []
      this.area_value = null
      if (this.region_value == null || this.region_value == '') {
        this.isDisabled = true
      } else {
        this.isDisabled = false
      }
      this.customer.name = null
      this.customer.code = null
      this.customer.group_type = null
      this.customer.is_active = null
      this.errors = []
    },
  },
}
</script>
<style>
.mt-20 {
  margin-top: 20px;
}
.mb-20 {
  margin-bottom: 20px;
}
</style>
