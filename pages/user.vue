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
            User
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
          <li class="breadcrumb-item text-muted">User</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of Users</h3>
          <div class="card-toolbar">
            <button
              type="button"
              class="btn btn-sm btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#modal"
              @click="add()"
            >
              Add User
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
                      v-if="order == 'nopeg' && by == 'asc'"
                      @click="sort('nopeg', 'desc')"
                      class="text-center"
                    >
                      Employee Number
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'nopeg' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Employee Number
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('nopeg', 'asc')"
                      class="text-center"
                    >
                      Employee Number <i class="fa-solid fa-sort"></i>
                    </th>

                    <th
                      v-if="order == 'email' && by == 'asc'"
                      @click="sort('email', 'desc')"
                      class="text-center"
                    >
                      Email
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'email' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Email
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th
                      v-else
                      @click="sort('email', 'asc')"
                      class="text-center"
                    >
                      Email <i class="fa-solid fa-sort"></i>
                    </th>

                    <th
                      v-if="order == 'unit' && by == 'asc'"
                      @click="sort('unit', 'desc')"
                      class="text-center"
                    >
                      Unit
                      <i class="fa-solid fa-sort-up" style="color: black"></i>
                    </th>
                    <th
                      v-else-if="order == 'unit' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                    >
                      Unit
                      <i class="fa-solid fa-sort-down" style="color: black"></i>
                    </th>
                    <th v-else @click="sort('unit', 'asc')" class="text-center">
                      Unit <i class="fa-solid fa-sort"></i>
                    </th>

                    <th class="text-center">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(users, transaction_index) in user.data"
                    :key="transaction_index"
                  >
                    <td class="text-center">
                      {{ user.from + transaction_index }}.
                    </td>
                    <td class="text-center">{{ users.name }}</td>
                    <td class="text-center">{{ users.nopeg }}</td>
                    <td class="text-center">{{ users.email }}</td>
                    <td class="text-center">{{ users.unit }}</td>

                    <td class="d-flex justify-content-center">
                      <button
                        class="btn btn-sm btn-light mx-2"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(users)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
                      <button
                        class="btn btn-sm btn-light"
                        v-on:click="remove(users.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="user.data.length < 1">
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
              class="col col-md-8 d-flex justify-content-end align-items-center"
            >
              <nav>
                <ul class="pagination">
                  <!-- Start pagination -->
                  <li
                    v-for="(link, link_index) in user.links"
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
      <div class="modal-dialog modal-dialog-centered modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h3 v-if="modal_create" class="modal-title">Add User</h3>
            <h3 v-else class="modal-title">Edit User</h3>

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
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Name</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="users.name"
                      :class="{
                        'is-invalid': errors.name,
                      }"
                      :disabled="!modal_create && users.guid !== null"
                    />
                    <span v-if="errors.name" class="error invalid-feedback">{{
                      errors.name[0]
                    }}</span>
                  </div>
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Username</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="users.username"
                      :class="{
                        'is-invalid': errors.username,
                      }"
                      :disabled="!modal_create && users.guid !== null"
                    />
                    <span
                      v-if="errors.username"
                      class="error invalid-feedback"
                      >{{ errors.username[0] }}</span
                    >
                  </div>
                  <div v-if="modal_create || users.guid === null" class="form-group mb-3">
                    <label class="form-label fw-bold">Password</label>
                    <input
                      type="password"
                      class="form-control"
                      v-model="users.password"
                      :class="{
                        'is-invalid': errors.password,
                      }"
                    />
                    <span
                      v-if="errors.password"
                      class="error invalid-feedback"
                      >{{ errors.password[0] }}</span
                    >
                  </div>
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Role</label>
                    <select
                      v-model="users.role_id"
                      class="form-select"
                      :class="{ 'is-invalid': errors.role_id }"
                    >
                      <option :value="null" disabled>Select Role</option>
                      <option
                        v-for="role_options in role_option"
                        :value="role_options.id"
                        :class="{
                          'is-invalid': errors.role_id,
                        }"
                      >
                        {{ role_options.name }}
                      </option>
                    </select>
                    <span
                      v-if="errors.role_id"
                      class="error invalid-feedback"
                      >{{ errors.role_id[0] }}</span
                    >
                  </div>
                </div>

                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Employee Number</label>
                    <input
                      type="number"
                      class="form-control"
                      v-model="users.nopeg"
                      :class="{
                        'is-invalid': errors.nopeg,
                      }"
                      :disabled="!modal_create && users.guid !== null"
                    />
                    <span v-if="errors.nopeg" class="error invalid-feedback">{{
                      errors.nopeg[0]
                    }}</span>
                  </div>
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Email</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="users.email"
                      :class="{
                        'is-invalid': errors.email,
                      }"
                      :disabled="!modal_create && users.guid !== null"
                    />
                    <span v-if="errors.email" class="error invalid-feedback">{{
                      errors.email[0]
                    }}</span>
                  </div>
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Unit</label>
                    <input
                      type="text"
                      class="form-control"
                      v-model="users.unit"
                      :class="{
                        'is-invalid': errors.unit,
                      }"
                    />
                    <span v-if="errors.unit" class="error invalid-feedback">{{
                      errors.unit[0]
                    }}</span>
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

<style>
.mt-20 {
  margin-top: 20px;
}
.mb-20 {
  margin-bottom: 20px;
}
.mb-10 {
  margin-bottom: 10px;
}
</style>

<script>
import debounce from 'lodash/debounce'
export default {
  layout: 'template',
  data() {
    return {
      role_user: this.$auth.user.role.name,
      user: {
        data: [],
        link: [],
      },
      users: {
        id: null,
        name: null,
        username: null,
        password: null,
        role_id: null,
        nopeg: null,
        email: null,
        unit: null,
      },
      modal_create: false,
      search: null,
      order: 'id',
      role_option: null,
      role_id: null,
      by: 'desc',
      paginate: '10',
      current_page: null,
      errors: {
        name: null,
        nopeg: null,
        username: null,
        password: null,
        role_id: null,
        email: null,
        unit: null,
      },
    }
  },
  created() {
    this.list()
    this.role()
  },
  watch: {
    search: debounce(function () {
      this.role()
      this.list()
    }, 500),
  },
  mounted() {
    this.redirectToDashboard()
  },
  methods: {
    redirectToDashboard() {
      if (this.role_user !== 'Administrator') {
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
      paginate = paginate || `/api/users`
      this.$axios
        .get(paginate, {
          params: {
            noAdmin: true,
            search: this.search,
            order: this.order,
            by: this.by,
            paginate: this.paginate,
          },
        })
        .then((response) => {
          this.user = response.data.data
          this.current_page = this.user.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    role() {
      this.loading()
      this.$axios
        .get(`api/role`)
        .then((response) => {
          this.role_option = response.data.data.data
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.user.last_page) {
        this.current_page = this.user.last_page
      }
      let url = new URL(this.user.first_page_url)
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
        .post('/api/users-create', {
          name: this.users.name,
          username: this.users.username,
          password: this.users.password,
          role_id: this.users.role_id,
          nopeg: this.users.nopeg,
          email: this.users.email,
          unit: this.users.unit,
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
    edit(users) {
      this.clearForm()
      this.$axios.get('/api/users-show/' + users.id).then((result) => {
        this.modal_create = false

        this.users.id = users.id
        this.users.name = users.name
        this.users.username = users.username
        this.users.password = users.password
        this.users.role_id = users.role_id
        this.users.nopeg = users.nopeg
        this.users.email = users.email
        this.users.unit = users.unit

        // Show guid
        this.users.guid = result.data.data.guid

        console.log(this.fail)
      })
    },
    update() {
      this.loading()

      this.$axios
        .put('/api/users-update/' + this.users.id, {
          name: this.users.name,
          username: this.users.username,
          password: this.users.password,
          role_id: this.users.role_id,
          nopeg: this.users.nopeg,
          email: this.users.email,
          unit: this.users.unit,
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
            this.$axios.delete('/api/users-delete/' + id).then((response) => {
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
      this.users.id = null
      this.users.name = null
      this.users.username = null
      this.users.password = null
      this.users.role_id = null
      this.users.nopeg = null
      this.users.email = null
      this.users.unit = null
      this.errors.name = null
      this.errors.username = null
      this.errors.password = null
      this.errors.role_id = null
      this.errors.nopeg = null
      this.errors.email = null
      this.errors.unit = null
    },
    closeModal() {
      document.getElementById('close_modal').click()
      this.clearForm()
    },
  },
}
</script>

<style>
#readOnly {
  background-color: #f0f0f5;
}
</style>
