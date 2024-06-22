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
            Product
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
          <li class="breadcrumb-item text-muted">Product</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container mb-10">
      <div class="card shadow-sm mt-5">
        <div class="card-header">
          <h3 class="card-title fw-bold">List of Products</h3>
          <div class="card-toolbar">
            <button
              type="button"
              class="btn btn-sm btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#modal"
              @click="add()"
            >
              Add product
            </button>
          </div>
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
                    <!-- Start Name sorting -->
                    <th v-if="order == 'name' && by == 'asc'" @click="sort('name', 'desc')" class="text-center">Name <i class="fa-solid fa-sort-up" style="color: black"></i></th>
                    <th v-else-if="order == 'name' && by == 'desc'" @click="sort('id', 'desc')" class="text-center">Name <i class="fa-solid fa-sort-down" style="color: black"></i></th>
                    <th v-else @click="sort('name', 'asc')" class="text-center">Name <i class="fa-solid fa-sort"></i></th>
                    <!-- End Name sorting -->
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
                    v-for="(p_product, p_product_index) in product.data"
                    :key="p_product_index"
                  >
                    <td class="text-center">
                      {{ product.from + p_product_index }}.
                    </td>
                    <td class="text-center">{{ p_product.name }}</td>
                    <td class="text-center">{{ p_product.description }}</td>

                    <td class="d-flex justify-content-center">
                      <button
                        class="btn btn-sm btn-light me-5"
                        data-bs-toggle="modal"
                        data-bs-target="#modal"
                        @click="edit(p_product)"
                      >
                        <i class="bi bi-pencil-square text-primary"></i>
                      </button>
                      <button
                        class="btn btn-sm btn-light"
                        v-on:click="remove(p_product.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="product.data.length < 1">
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
            <div class="col col-md-8 d-flex justify-content-end align-items-center">
              <nav aria-label="Page navigation example">
                <ul class="pagination">
                  <!-- Start pagination -->
                  <li v-for="(link, link_index) in product.links" :key="link_index" class="page-item" :class="{ disabled: !link.url, active: link.active }">
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
            <h3 v-if="modal_create" class="modal-title">Add product</h3>
            <h3 v-else class="modal-title">Edit product</h3>

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
                <label class="form-label fw-bold">Name</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="p_product.name"
                  :class="{
                    'is-invalid': errors.name,
                  }"
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
                  v-model="p_product.description"
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
      role: this.$auth.user.role.name,
      product: {
        data: [],
        link: [],
      },
      p_product: {
        id: null,
        name: null,
        description: null,
      },
      modal_create: false,
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,
      errors: {
        name: null,
        description: null,
      },
    }
  },
  created() {
    this.list()
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
      this.order = order;
      this.by = by;
      this.list();
    },
    list(paginate) {
      this.loading()
      paginate = paginate || `/api/product`
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
          this.product = response.data.data
          this.current_page = this.product.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.product.last_page) {
        this.current_page = this.product.last_page
      }
      let url = new URL(this.product.first_page_url)
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
        .post('/api/product-create', {
          name: this.p_product.name,
          description: this.p_product.description,
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
    edit(p_product) {
      this.modal_create = false
      this.p_product.id = p_product.id
      this.p_product.name = p_product.name
      this.p_product.description = p_product.description
    },
    update() {
      this.loading()

      this.$axios
        .put('/api/product-update/' + this.p_product.id, {
          name: this.p_product.name,
          description: this.p_product.description,
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
            this.$axios.delete('/api/product-delete/' + id).then((response) => {
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
      this.p_product.id = null
      this.p_product.name = null
      this.p_product.description = null
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
