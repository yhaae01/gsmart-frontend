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
          <li class="breadcrumb-item text-muted">View Prospect PBTH</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container-fluid mb-10">
      <div class="card shadow-sm mt-5">
        <div class="row">
          <div class="col-md-5 p-16">
            <span class="badge text-muted text-bg-light d-block text-start mt-2"
              >PBTH</span
            >
            <h2 class="mt-1">{{ product }}</h2>
            <div class="text-muted fw-semibold fs-5">
              Project for {{ registration }}
            </div>
            <div class="row mt-5">
              <div class="col-6">
                <div class="border-dashed rounded p-4">
                  <h1 class="fw-bolder mb-0 fs-5">
                    USD {{ formatNumber(market_share) }}
                  </h1>
                  <p class="mb-0 fw-bold text-gray-500">Sales RKAP</p>
                </div>
              </div>
              <div class="col-6">
                <div class="border-dashed rounded p-4" v-if="sales_plan">
                  <h1 class="fw-bolder mb-0 fs-5">
                    USD {{ formatNumber(sales_plan) }}
                  </h1>
                  <p class="mb-0 fw-bold text-gray-500">Sales Plan</p>
                </div>
                <div class="border-dashed rounded p-4" v-else>
                  <h1 class="fw-bolder mb-0 fs-5">USD 0.00</h1>
                  <p class="mb-0 fw-bold text-gray-500">Sales Plan</p>
                </div>
              </div>
            </div>
            <div class="row mt-5">
              <div class="col-6">
                <div class="border-dashed rounded p-4" v-if="deviation">
                  <h1 class="fw-bolder mb-0 fs-5">
                    USD {{ formatNumber(deviation) }}
                  </h1>
                  <p class="mb-0 fw-bold text-gray-500">Deviation</p>
                </div>
                <div class="border-dashed rounded p-4" v-else>
                  <h1 class="fw-bolder mb-0 fs-5">USD 0.00</h1>
                  <p class="mb-0 fw-bold text-gray-500">Deviation</p>
                </div>
              </div>
            </div>
          </div>
          <div class="col-md-7 container">
            <div class="row mt-15 mb-2">
              <div class="col-6">
                <div class="fs-3">Contact Person</div>
              </div>
              <div class="col-6 text-end">
                <div
                  type="button"
                  class="btn btn-sm text-light"
                  data-bs-toggle="modal"
                  data-bs-target="#contactPersonModal"
                  @click="clearForm()"
                  style="background-color: #1bc5bd"
                >
                  <i class="fa-solid fa-user-plus text-light"></i>
                  Add Contact Person
                </div>
              </div>
              <div
                class="modal fade"
                tabindex="-1"
                id="contactPersonModal"
                data-bs-backdrop="static"
              >
                <div class="modal-dialog modal-dialog-centered">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h3 class="modal-title">Add Contact Person</h3>

                      <!--begin::Close-->
                      <div
                        class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                        data-bs-dismiss="modal"
                        aria-label="Close"
                        id="close_modal_contact_person"
                      >
                        <span class="svg-icon svg-icon-1">
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
                            v-model="contact_person.name"
                            class="form-control"
                            :class="{ 'is-invalid': errors.name }"
                          />
                          <span
                            v-if="errors.name"
                            class="error invalid-feedback"
                            >{{ errors.name[0] }}</span
                          >
                        </div>

                        <div class="form-group mb-3">
                          <label class="form-label fw-bold">Email</label>
                          <input
                            type="email"
                            class="form-control"
                            v-model="contact_person.email"
                            :class="{
                              'is-invalid': errors.email,
                            }"
                          />
                          <span
                            v-if="errors.email"
                            class="error invalid-feedback"
                            >{{ errors.email[0] }}</span
                          >
                        </div>
                        <div class="form-group mb-3">
                          <label class="form-label fw-bold">Phone</label>
                          <input
                            type="number"
                            class="form-control"
                            v-model="contact_person.phone"
                            :class="{
                              'is-invalid': errors.phone,
                            }"
                          />
                          <span
                            v-if="errors.phone"
                            class="error invalid-feedback"
                            >{{ errors.phone[0] }}</span
                          >
                        </div>
                        <div class="form-group mb-3">
                          <label class="form-label fw-bold">Address</label>
                          <input
                            type="text"
                            class="form-control"
                            v-model="contact_person.address"
                            :class="{
                              'is-invalid': errors.address,
                            }"
                          />
                          <span
                            v-if="errors.address"
                            class="error invalid-feedback"
                            >{{ errors.address[0] }}</span
                          >
                        </div>
                        <div class="form-group mb-3">
                          <label class="form-label fw-bold">Title</label>
                          <input
                            type="text"
                            class="form-control"
                            v-model="contact_person.title"
                            :class="{
                              'is-invalid': errors.title,
                            }"
                          />
                          <span
                            v-if="errors.title"
                            class="error invalid-feedback"
                            >{{ errors.title[0] }}</span
                          >
                        </div>
                        <div class="row mt-10">
                          <div class="col">
                            <button
                              type="button"
                              class="btn btn-light"
                              @click="closeModal()"
                            >
                              Back
                            </button>
                          </div>
                          <div class="col d-flex justify-content-end">
                            <button
                              type="button"
                              class="btn btn-primary"
                              @click="createContactPerson()"
                            >
                              Save
                            </button>
                          </div>
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="table-responsive">
              <table class="table table-row-bordered table-row-gray-200 gy-4">
                <thead>
                  <tr class="fw-bold fs-6 text-gray-800">
                    <th class="text-start text-uppercase text-muted">Name</th>
                    <th class="text-start text-uppercase text-muted">Email</th>
                    <th class="text-start text-uppercase text-muted">Phone</th>
                    <th class="text-start text-uppercase text-muted">
                      Address
                    </th>
                    <th class="text-start text-uppercase text-muted">Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(data, contact_person_index) in contact_persons"
                    :key="contact_person_index"
                  >
                    <td class="text-start">{{ data.name }}</td>
                    <td class="text-start">{{ data.email }}</td>
                    <td class="text-start">{{ data.phone }}</td>
                    <td class="text-start">{{ data.address }}</td>
                    <td class="text-start">
                      <button
                        class="btn btn-sm btn-light"
                        v-on:click="removeContactPerson(data.id)"
                      >
                        <i class="bi bi-trash-fill text-primary"></i>
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>

      <div class="card my-10">
        <div class="card-header">
          <h3 class="card-title fw-bold">Select Input Type</h3>
        </div>
        <div class="card-body">
          <div class="col-6">
            <multiselect
              v-model="inputOptionsValue"
              :options="inputOptions"
              :allow-empty="false"
              deselect-label="Selected"
              @select="clearValue()"
            ></multiselect>
          </div>
        </div>
      </div>

      <div class="mb-10" v-if="inputOptionsValue == 'Input Convidence Level'">
        <div class="card shadow-sm mt-5">
          <div class="card-header">
            <h3 class="card-title fw-bold">Convidence Level</h3>
          </div>
          <div class="card-body">
            <div class="row">
              <div class="col-md-2">
                <div class="input-group">
                  <input
                    type="text"
                    class="form-control form-control-solid"
                    name="convidence"
                    v-model="convidence"
                  />
                  <div class="input-group-append">
                    <span class="input-group-text">%</span>
                  </div>
                </div>
              </div>
              <div class="col-md-10">
                <button
                  @click="calculateSalesPlan(convidence)"
                  class="btn btn-primary"
                  :disabled="
                    convidence > 100 ||
                    convidence < 0 ||
                    convidence == null ||
                    convidence == ''
                  "
                >
                  Calculate
                </button>
                <button
                  class="btn btn-primary ms-3"
                  @click="createSalesPlan()"
                  :disabled="
                    total_sales_plan == null ||
                    total_sales_plan == '' ||
                    total_sales_plan == 0
                  "
                >
                  Save
                </button>
              </div>
              <div class="mt-3" v-if="convidence > 100 || convidence < 0">
                <span class="text-danger fs-4">
                  Convidence Level must be between 1-100
                </span>
              </div>
            </div>
          </div>

          <div class="card shadow-sm mt-5">
            <div class="card-header">
              <h3 class="card-title fw-bold">Flights Hour Target</h3>
            </div>
            <div class="table-responsive container">
              <table class="table table-row-bordered table-row-gray-200 gy-4">
                <thead>
                  <tr class="fw-bold fs-6 text-gray-800">
                    <th class="text-center">Month</th>
                    <th class="text-center">Target FH</th>
                    <th class="text-center">Target Rate</th>
                    <th class="text-center">Convidence Level</th>
                    <th class="text-center">Sales Plan</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(data, pbth_index) in pbth" :key="pbth_index">
                    <td class="text-center">{{ data.month }}</td>
                    <td class="text-center">{{ data.flight_hour }} FH</td>
                    <td class="text-center" style="color: #50cd89">
                      USD {{ formatNumber(data.rate) }}
                    </td>
                    <td class="text-center" v-if="data.planFH">
                      {{ data.planFH }} %
                    </td>
                    <td class="text-center" v-else>0 %</td>
                    <td
                      class="text-center"
                      style="color: #50cd89"
                      v-if="data.value"
                    >
                      USD {{ formatNumber(data.value) }}
                    </td>
                    <td class="text-center" style="color: #50cd89" v-else>
                      USD 0
                    </td>
                  </tr>
                  <tr>
                    <td class="text-center"></td>
                    <td class="text-center"></td>
                    <td class="text-center"></td>
                    <td class="text-center fs-3 fw-bold">Total Sales Plan</td>
                    <td class="text-center fs-3 fw-bold" style="color: #50cd89">
                      USD
                      {{
                        formatNumber(
                          isNaN(total_sales_plan) ? 0 : total_sales_plan
                        )
                      }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
      <div
        class="mb-10"
        v-else-if="inputOptionsValue == 'Input Convidence Flight Hour'"
      >
        <div class="card shadow-sm mt-5">
          <div class="card-header">
            <h3 class="card-title fw-bold">Convidence Flight Hour</h3>
          </div>
          <div class="card-body">
            <div class="row">
              <div class="col-md-2">
                <div class="input-group">
                  <input
                    type="text"
                    class="form-control form-control-solid"
                    name="convidence"
                    v-model="convidence"
                  />
                  <div class="input-group-append">
                    <span class="input-group-text">FH</span>
                  </div>
                </div>
              </div>
              <div class="col-md-10">
                <button
                  @click="calculateSalesPlanFH(convidence)"
                  class="btn btn-primary"
                  :disabled="convidence == null || convidence == ''"
                >
                  Calculate
                </button>
                <button
                  class="btn btn-primary ms-3"
                  @click="createSalesPlan()"
                  :disabled="
                    total_sales_plan == null ||
                    total_sales_plan == '' ||
                    total_sales_plan == 0
                  "
                >
                  Save
                </button>
              </div>
            </div>
          </div>

          <div class="card shadow-sm mt-5">
            <div class="card-header">
              <h3 class="card-title fw-bold">Flights Hour Target</h3>
            </div>
            <div class="table-responsive container">
              <table class="table table-row-bordered table-row-gray-200 gy-4">
                <thead>
                  <tr class="fw-bold fs-6 text-gray-800">
                    <th class="text-center">Month</th>
                    <th class="text-center">Target FH</th>
                    <th class="text-center">Target Rate</th>
                    <th class="text-center">Convidence Flight Hour</th>
                    <th class="text-center">Sales Plan</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(data, pbth_index) in pbth" :key="pbth_index">
                    <td class="text-center">{{ data.month }}</td>
                    <td class="text-center">{{ data.flight_hour }} FH</td>
                    <td class="text-center" style="color: #50cd89">
                      USD {{ formatNumber(data.rate) }}
                    </td>
                    <td class="text-center" v-if="data.planFH">
                      {{ data.planFH }}
                    </td>
                    <td class="text-center" v-else>0</td>
                    <td
                      class="text-center"
                      style="color: #50cd89"
                      v-if="data.value"
                    >
                      USD {{ formatNumber(data.value) }}
                    </td>
                    <td class="text-center" style="color: #50cd89" v-else>
                      USD 0
                    </td>
                  </tr>
                  <tr>
                    <td class="text-center"></td>
                    <td class="text-center"></td>
                    <td class="text-center"></td>
                    <td class="text-center fs-3 fw-bold">Total Sales Plan</td>
                    <td class="text-center fs-3 fw-bold" style="color: #50cd89">
                      USD
                      {{
                        formatNumber(
                          isNaN(total_sales_plan) ? 0 : total_sales_plan
                        )
                      }}
                    </td>
                  </tr>
                </tbody>
              </table>
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
      deviation: this.market_share - this.sales_plan,
      customer_options: [],
      pbth: [],
      sales_plan: [],
      registration: null,
      customer_image: [],
      market_share: null,
      contact_persons: [],
      contact_person: {
        name: null,
        email: null,
        address: null,
        phone: null,
        customer_id: null,
        title: null,
      },
      modal_create: false,
      convidence: null,
      errors: {
        name: null,
        email: null,
        address: null,
        phone: null,
        customer_id: null,
        title: null,
      },
      customer_id: null,
      total_sales_plan: null,
      pbth_length: null,
      initial: null,
      inputOptionsValue: 'Input Convidence Level',
      inputOptions: ['Input Convidence Flight Hour', 'Input Convidence Level'],
      product: null,
    }
  },
  created() {
    this.loading()
    this.listContactPersons()
    this.listPBTH()
    this.listCustomer()
    this.checkRole()
  },
  methods: {
    clearValue() {
      this.total_sales_plan = null
      this.convidence = null
      this.pbth.forEach((element) => {
        element.planFH = 0
        element.value = 0
      })
    },
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
      toastr.error('Sorry, You Are Not Allowed to Access Prospect PBTH Page!')
    },
    PBTHMessage() {
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
      toastr.error('Sorry, Prospect Has Been Picked Up!')
    },
    checkRole() {
      if (this.role == 'AMS' || this.role == 'AM' || this.role == 'Administrator') {
      } else {
        this.$router.push('/my-prospect')
        this.authMessage()
      }
    },
    formatNumber(value) {
      let val = (value / 1).toFixed(2).replace(',', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
    listContactPersons() {
      this.$axios.get('api/contact-person').then((response) => {
        this.contact_persons = response.data.data.data
      })
    },
    listCustomer() {
      this.$axios
        .get('api/customer-group', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.customer_options = response.data.data.data
        })
    },
    listContactPersons() {
      this.$axios.get('api/contact-person').then((response) => {
        this.contact_persons = response.data.data.data
      })
    },
    listPBTH() {
      this.$axios
        .get(`api/prospect-pbth/${this.$route.query.id}`)
        .then((response) => {
          // Data Product
          this.product = response.data.data.prospect[0].product.name
          // Data PBTH
          this.pbth = response.data.data.prospect
          // Data Customer
          this.customer_name = response.data.data.customer.name
          // Get Customer ID
          this.customer_id = response.data.data.customer.id
          // Registration Format
          this.registration = response.data.data.registration
          // Sales Plan Value
          this.sales_plan = response.data.data.sales_plan
          // Market Share Value
          this.market_share = response.data.data.market_share
          // Deviation Value
          this.deviation = response.data.data.deviation
          this.pbth.forEach((element) => {
            element.planFH = 0
            element.value = 0
          })
          if (this.sales_plan !== 0) {
            this.PBTHMessage()
            this.$router.push('/my-prospect')
          }
        })
        .catch((error) => {
          if (error.response.status == 404) {
            toastr.error(error.response.data.message)
            this.$router.push({
              name: 'my-prospect',
            })
          } else if (error.response.status == 403) {
            toastr.error(error.response.data.message)
            this.$router.push({
              name: 'my-prospect',
            })
          }
        })
    },
    calculateSalesPlan() {
      let convidence = this.convidence / 100
      this.pbth_length = this.pbth.length
      this.pbth.forEach((element) => {
        element.month = element.month + ' '
        element.planFH = this.convidence
        let TargetFHXConvidenceLevel = element.flight_hour * convidence
        element.value = TargetFHXConvidenceLevel * element.rate
      })
      this.total_sales_plan = 0
      for (let i = 0; i < this.pbth_length; i++) {
        this.total_sales_plan += this.pbth[i].value
      }
    },
    calculateSalesPlanFH() {
      let convidence = this.convidence / 100
      this.pbth_length = this.pbth.length
      this.pbth.forEach((element) => {
        element.month = element.month + ' '
        element.planFH = this.convidence
        let TFHxRate = element.flight_hour * element.rate
        let CFHxTFH = this.convidence / element.flight_hour
        element.value = TFHxRate * CFHxTFH
      })
      this.total_sales_plan = 0
      for (let i = 0; i < this.pbth_length; i++) {
        this.total_sales_plan += this.pbth[i].value
      }
    },
    createSalesPlan() {
      Swal.fire({
        title: 'Submit Confirmation',
        text: 'This action will created Sales Plan data in a year, are you sure?',
        icon: 'question',
        showCancelButton: true,
        confirmButtonText: 'Yes, create Sales!',
        cancelButtonText: 'No, cancel!',
      }).then((result) => {
        if (result.isConfirmed) {
          this.onLoad()
          this.$axios
            .post('/api/sales-create-pbth', {
              prospect_id: this.$route.query.id,
              pbth: this.pbth,
            })
            .then((response) => {
              Swal.close()
              toastr.success(response.data.message)
              this.$router.push('/my-prospect')
            })
            .catch((error) => {
              Swal.close()
              this.errors = error.response.data.errors
              toastr.error(error.response.data.message)
            })
        }
      })
    },
    submit() {
      this.createContactPerson()
    },
    createContactPerson() {
      this.loading()
      this.$axios
        .post('/api/contact-person-create', {
          name: this.contact_person.name,
          phone: this.contact_person.phone,
          email: this.contact_person.email,
          address: this.contact_person.address,
          customer_id: this.customer_id,
          title: this.contact_person.title,
        })
        .then((response) => {
          toastr.success(response.data.message)
          this.listContactPersons()
          this.closeModal()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          }
        })
    },
    clearForm() {
      this.contact_person.name = null
      this.contact_person.email = null
      this.contact_person.address = null
      this.contact_person.phone = null
      this.contact_person.customer_id = null
      this.contact_person.title = null
      this.errors.name = null
      this.errors.email = null
      this.errors.phone = null
      this.errors.address = null
      this.errors.customer_id = null
    },
    closeModal() {
      document.getElementById('close_modal_contact_person').click()
      this.clearForm()
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
            this.$axios
              .delete('/api/contact-person-delete/' + id)
              .then((response) => {
                toastr.success(response.data.message)
                this.listContactPersons()
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
    onLoad() {
      Swal.fire({
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
        allowEscapeKey: false,
        allowEnterKey: false,
      })
    },
  },
}
</script>
