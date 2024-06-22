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
              fw-bold
            "
          >
            Permission
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
          <li class="breadcrumb-item text-muted">Permission</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of Permissions</h3>
        </div>
        <div class="card-body">
          <div class="row d-flex align-items-center">
            <!--begin::Input group-->
            <div class="position-relative me-md-2">
              <!--begin::Svg Icon | path: icons/duotune/general/gen021.svg-->
              <span
                class="
                  svg-icon svg-icon-3 svg-icon-gray-500
                  position-absolute
                  top-50
                  translate-middle
                  ms-6
                "
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
                    <!-- Start name sorting -->
                    <th v-if="order == 'name' && by == 'asc'" @click="sort('name', 'desc')" class="text-center">Name <i class="fa-solid fa-sort-up" style="color: black"></i></th>
                    <th v-else-if="order == 'name' && by == 'desc'" @click="sort('id', 'desc')" class="text-center">Name <i class="fa-solid fa-sort-down" style="color: black"></i></th>
                    <th v-else @click="sort('name', 'asc')" class="text-center">Name <i class="fa-solid fa-sort"></i></th>
                    <!-- End name sorting -->
                    <!-- Start description sorting -->
                    <th v-if="order == 'description' && by == 'asc'" @click="sort('description', 'desc')" class="text-center">Description <i class="fa-solid fa-sort-up" style="color: black"></i></th>
                    <th v-else-if="order == 'description' && by == 'desc'" @click="sort('id', 'desc')" class="text-center">Description <i class="fa-solid fa-sort-down" style="color: black"></i></th>
                    <th v-else @click="sort('description', 'asc')" class="text-center">Description <i class="fa-solid fa-sort"></i></th>
                    <!-- End description sorting -->
                    <th class="text-center">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(permission, permission_index) in permissions.data"
                    :key="permission_index"
                  >
                    <td class="text-center">
                      {{ permissions.from + permission_index }}.
                    </td>
                    <td class="text-center">{{ permission.name }}</td>
                    <td class="text-center">{{ permission.description }}</td>
                    <td class="text-center">
                      <button
                        class="btn btn-sm btn-light"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(permission)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
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
            <div class="col col-md-8 d-flex justify-content-end align-items-center">
              <nav>
                <ul class="pagination">
                  <!-- Start pagination -->
                  <li v-for="(link, link_index) in permissions.links" :key="link_index" class="page-item" :class="{ disabled: !link.url, active: link.active }">
                    <a href="javascript:void(0)" @click="list(link.url)" class="page-link">
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
            <h3 v-if="modal_create" class="modal-title">Add Permission</h3>
            <h3 v-else class="modal-title">Edit Permission</h3>

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
            <form @submit.prevent="update">
              <div class="form-group mb-3">
                <label class="form-label fw-bold">Name</label>
                <input
                  type="text"
                  class="form-control"
                  disabled
                  v-model="permission.name"
                />
                <span v-if="errors.name" class="error invalid-feedback">{{
                  errors.name[0]
                }}</span>
              </div>
              <div class="form-group mb-3">
                <label class="form-label fw-bold">Description</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="permission.description"
                  :class="{
                    'is-invalid': errors.description,
                  }"
                />
                <span
                  v-if="errors.description"
                  class="error invalid-feedback"
                  >{{ errors.description[0] }}</span
                >
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
  name: 'PermissionPage',
  data() {
    return {
      role: this.$auth.user.role.name,
      permissions: {
        data: [],
        link: [],
      },
      permission: {
        id: null,
        name: null,
        description: null,
        permission: null,
      },
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,
      errors: {
        name: null,
        description: null,
      },
      modal_create: false,
    }
  },
  created() {
    this.list()
  },
  mounted() {
    this.redirectToDashboard()
  },
  watch: {
    search: debounce(function () {
      this.list()
    }, 500),
  },
  methods: {
    redirectToDashboard() {
      if (this.role !== 'Administrator') {
        this.$router.push('/');
      }
    },
    sort(order, by) {
      this.order = order;
      this.by = by;
      this.list();
    },
    list(paginate) {
      this.loading()
      paginate = paginate || `/api/permission`
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
          this.permissions = response.data.data
          this.current_page = this.permissions.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.permissions.last_page) {
        this.current_page = this.permissions.last_page
      }
      let url = new URL(this.permissions.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.list(new_url)
    }, 500),
    add() {
      this.modal_create = true
    },
    edit(permission) {
      this.modal_create = false
      this.permission.id = permission.id
      this.permission.name = permission.name
      this.permission.description = permission.description
    },
    update() {
      this.loading()
      this.$axios
        .put('/api/permission-update/' + this.permission.id, {
          description: this.permission.description,
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
      this.permission.id = null
      this.permission.name = null
      this.permission.description = null
      this.errors.name = null
      this.errors.description = null
    },
    closeModal() {
      document.getElementById('close_modal').click()
      this.clearForm()
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
