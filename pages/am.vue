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
            AM
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
          <li class="breadcrumb-item text-muted">AM</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of AM</h3>
          <div class="card-toolbar">
            <button
              type="button"
              class="btn btn-sm btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#modal"
              @click="add()"
            >
              Add AM
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
                      User
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'user' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      User
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th v-else @click="sort('user', 'asc')" class="text-center">
                      User <i class="fa-solid fa-sort"></i>
                    </th>
                    <!-- End user sorting -->

                    <!-- Start area sorting -->
                    <th class="text-center">Area</th>
                    <!-- End area sorting -->

                    <th class="text-center">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(p_am, p_am_index) in am.data" :key="p_am_index">
                    <td class="text-center">{{ am.from + p_am_index }}.</td>
                    <td class="text-center">{{ p_am.initial }}</td>
                    <td class="text-center">{{ p_am.user.name }}</td>
                    <td class="text-center">{{ p_am.area.name }}</td>
                    <td class="d-flex justify-content-center">
                      <button
                        class="btn btn-sm btn-light me-5"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(p_am)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
                      <button
                        class="btn btn-sm btn-light"
                        v-on:click="remove(p_am.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="am.data.length < 1">
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
                    v-for="(link, link_index) in am.links"
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
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h3 v-if="modal_create" class="modal-title">Add AM</h3>
            <h3 v-else class="modal-title">Edit AM</h3>

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
                  v-model="p_am.initial"
                  :class="{
                    'is-invalid': errors.initial,
                  }"
                />
                <span v-if="errors.initial" class="error invalid-feedback">{{
                  errors.initial[0]
                }}</span>
              </div>
              <div class="form-group mb-3">
                <label class="form-label fw-bold">User</label>
                <select
                  v-model="p_am.user_id"
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
                <label class="form-label fw-bold">Area</label>
                <select
                  v-model="p_am.area_id"
                  class="form-select"
                  :class="{ 'is-invalid': errors.area_id }"
                >
                  <option
                    v-for="area_options in areas"
                    :value="area_options.id"
                  >
                    {{ area_options.name }}
                  </option>
                </select>
                <span v-if="errors.area_id" class="error invalid-feedback">{{
                  errors.area_id[0]
                }}</span>
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
  data() {
    return {
      role: this.$auth.user.role.name,
      am: {
        data: [],
        link: [],
      },
      users: [],
      areas: [],
      p_am: {
        id: null,
        initial: null,
        area_id: null,
        user_id: null,
      },
      modal_create: false,
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,
      errors: {
        initial: null,
        area_id: null,
        user_id: null,
      },
    }
  },
  created() {
    this.list()
    this.user()
    this.area()
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
        this.$router.push('/');
      }
    },
    sort(order, by) {
      this.order = order
      this.by = by
      this.list()
    },
    list(paginate) {
      this.loading()
      paginate = paginate || `/api/am`
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
          this.am = response.data.data

          this.current_page = this.am.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.am.last_page) {
        this.current_page = this.am.last_page
      }
      let url = new URL(this.am.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.list(new_url)
    }, 500),
    submit() {
      if (this.modal_create) {
        this.create()
      } else {
        this.update()
      }
    },
    add() {
      this.modal_create = true
    },
    create() {
      this.loading()
      this.$axios
        .post('/api/am-create', {
          initial: this.p_am.initial,
          area_id: this.p_am.area_id,
          user_id: this.p_am.user_id,
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
    edit(p_am) {
      this.modal_create = false
      this.p_am.id = p_am.id
      this.p_am.initial = p_am.initial
      this.p_am.area_id = p_am.area_id
      this.p_am.user_id = p_am.user_id
    },
    update() {
      this.loading()

      this.$axios
        .put('/api/am-update/' + this.p_am.id, {
          initial: this.p_am.initial,
          area_id: this.p_am.area_id,
          user_id: this.p_am.user_id,
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
    remove(id) {
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
            this.$axios.delete('/api/am-delete/' + id).then((response) => {
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
      this.p_am.id = null
      this.p_am.initial = null
      this.p_am.area_id = null
      this.p_am.user_id = null
      this.errors.initial = null
      this.errors.area_id = null
      this.errors.user_id = null
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
    area() {
      this.$axios
        .get('/api/area')
        .then((response) => {
          this.areas = response.data.data.data
        })
        .catch((error) => console.log(error))
    },
  },
}
</script>
