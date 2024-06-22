<template>
  <div>
    <div class="container-fluid mt-10 mb-20">
      <div class="row mb-6">
        <div class="col-lg-6 col-sm-12">
          <h3 class="mt-3">Dashboard Sales Plan</h3>
        </div>
        <div class="col-lg-6 col-sm-12 d-flex justify-content-end">
          <button
            type="button"
            class="btn btn-info btn-sm me-2"
            data-bs-toggle="modal"
            data-bs-target="#filterdate"
          >
            <i class="fa-solid fa-calendar-days"></i>
            Filter Date
          </button>
          <button
            data-bs-toggle="modal"
            data-bs-target="#uploadFile"
            class="btn btn-sm btn-success me-2"
            v-if="role === 'Administrator'"
          >
            <i class="fa-solid fa-file-excel"></i>
            Import Excel
          </button>

          <NuxtLink
            class="btn btn-sm btn-primary"
            to="/my-salesplan-table"
            exact-active-class="active"
          >
            <i class="fa-solid fa-table-list"></i>
            <span>Sales Plan Table</span>
          </NuxtLink>
        </div>
      </div>

      <div class="row mb-6">
        <!-- <div class="col-lg-3">
          <multiselect
            v-model="selectedYear"
            :options="yearOptions"
            track-by="value"
            placeholder="Select Year"
            label="text"
            id="year"
            @input="onYearSelect"
          ></multiselect>
        </div> -->
        <div v-if="role != 'AMS'" class="col-lg-4">
          <multiselect
            v-model="ams"
            :options="ams_option"
            open-direction="bottom"
            placeholder="Filter AMS"
            label="initial"
            :multiple="true"
            :close-on-select="false"
            :clear-on-select="false"
            :preserve-search="true"
            track-by="initial"
            :preselect-first="true"
            :class="{ 'is-invalid': errors.ams }"
            @input="handleMultiselectAms"
          ></multiselect>
        </div>
        <div :class="{ 'col-lg-4': role != 'AMS', 'col-lg-6': role == 'AMS' }">
          <multiselect
            v-model="customer"
            :options="customer_option"
            open-direction="bottom"
            placeholder="Filter Customer"
            label="label"
            :multiple="true"
            :close-on-select="false"
            :clear-on-select="false"
            :preserve-search="true"
            track-by="name"
            :preselect-first="true"
            :class="{ 'is-invalid': errors.customer }"
            @input="handleMultiselectCustomer"
          ></multiselect>
        </div>
        <div :class="{ 'col-lg-4': role != 'AMS', 'col-lg-6': role == 'AMS' }">
          <multiselect
            v-model="product"
            :options="product_option"
            open-direction="bottom"
            placeholder="Filter Product"
            label="name"
            :multiple="true"
            :close-on-select="false"
            :clear-on-select="false"
            :preserve-search="true"
            track-by="name"
            :preselect-first="true"
            :class="{ 'is-invalid': errors.product }"
            @input="handleMultiselectProduct"
          ></multiselect>
        </div>
      </div>
      <!-- Row Total -->
      <div class="card shadow-sm mb-5">
        <div class="card-body">
          <div class="row" v-if="sales">
            <div class="col-lg my-1">
              <nuxtlink>
                <!-- <nuxtlink @click="filterTotal()"> -->
                <div class="border-dashed p-4">
                  <p class="text-muted" id="fontSm">Total Target</p>
                  <h4>USD {{ formatPrice(sales.totalTarget) }}</h4>
                </div>
              </nuxtlink>
            </div>
            <div class="col-lg my-1">
              <nuxtlink>
                <!-- <nuxtlink @click="filterOpen()"> -->
                <div class="border-dashed p-4">
                  <p class="text-muted" id="fontSm">Total Open</p>
                  <h4>USD {{ formatPrice(sales.totalOpen) }}</h4>
                </div>
              </nuxtlink>
            </div>
            <div class="col-lg my-1">
              <nuxtlink>
                <!-- <nuxtlink @click="filterClosedIn()"> -->
                <div class="border-dashed p-4">
                  <p class="text-muted" id="fontSm">Total Closed Sales</p>
                  <h4>USD {{ formatPrice(sales.totalClosedSales) }}</h4>
                </div>
              </nuxtlink>
            </div>
            <div class="col-lg my-1">
              <nuxtlink>
                <!-- <nuxtlink @click="filterClosed()"> -->
                <div class="border-dashed p-4">
                  <p class="text-muted" id="fontSm">Total Closed In</p>
                  <h4>USD {{ formatPrice(sales.totalClosedIn) }}</h4>
                </div>
              </nuxtlink>
            </div>
            <div class="col-lg my-1">
              <nuxtlink>
                <!-- <nuxtlink @click="filterCancel()"> -->
                <div class="border-dashed p-4">
                  <p class="text-muted" id="fontSm">Total Cancel</p>
                  <h4>USD {{ formatPrice(sales.totalCancel) }}</h4>
                </div>
              </nuxtlink>
            </div>
          </div>
        </div>
      </div>
      <!-- End row total -->

      <!-- Row 2 -->
      <div class="row mt-10">
        <!-- Level 4 -->
        <div class="col-lg-3 col-md-6 mt-10">
          <div class="card card-stretch-50 shadow mb-5">
            <div
              class="position-absolute top-0 start-50 translate-middle d-flex justify-content-center rounded"
              id="bgPurple2"
            >
              <h1 class="mt-5" id="textPurple">4</h1>
            </div>
            <div class="card-body mt-10">
              <div class="text-center">
                <h2 id="textPurple" v-if="sales">
                  USD {{ formatPrice(sales.level4.total) }}
                </h2>
                <p class="text-muted">Awareness</p>
              </div>
              <div class="d-grid gap-2">
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level4Open()"
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span
                      id="btnGreen"
                      style="margin-left: -10px"
                      v-if="sales"
                      >{{ formatPrice(sales.level4.countOpen) }}</span
                    >
                    <span id="textGreen" v-if="sales"
                      >USD {{ formatPrice(sales.level4.open) }}</span
                    >
                  </div>
                  <div
                    class="d-flex align-items-center justify-content-end"
                    style="margin-left: -15px"
                  >
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Open</span
                    >
                  </div>
                </nuxtlink>
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level4Cancel()"
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span id="btnRed" style="margin-left: -10px" v-if="sales">{{
                      formatPrice(sales.level4.countCancel)
                    }}</span>
                    <span id="textRed" v-if="sales"
                      >USD {{ formatPrice(sales.level4.cancel) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Cancel</span
                    >
                  </div>
                </nuxtlink>
              </div>
            </div>
            <div class="card-footer border-0" id="cardFooter"></div>
          </div>
        </div>

        <!-- Level 3 -->
        <div class="col-lg-3 col-md-6 mt-10">
          <div class="card card-stretch-50 shadow mb-5">
            <div
              class="position-absolute top-0 start-50 translate-middle d-flex justify-content-center rounded"
              id="bgGold2"
            >
              <h1 class="mt-5" id="textGold">3</h1>
            </div>
            <div class="card-body mt-10">
              <div class="text-center">
                <h2 id="textGold" v-if="sales">
                  USD {{ formatPrice(sales.level3.total) }}
                </h2>
                <p class="text-muted">Opportunity</p>
              </div>
              <div class="d-grid gap-2">
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level3Open()"
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span
                      id="btnGreen"
                      style="margin-left: -10px"
                      v-if="sales"
                      >{{ formatPrice(sales.level3.countOpen) }}</span
                    >
                    <span id="textGreen" v-if="sales"
                      >USD {{ formatPrice(sales.level3.open) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Open</span
                    >
                  </div>
                </nuxtlink>
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level3Cancel()"
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span id="btnRed" style="margin-left: -10px" v-if="sales">{{
                      formatPrice(sales.level3.countCancel)
                    }}</span>
                    <span id="textRed" v-if="sales"
                      >USD {{ formatPrice(sales.level3.cancel) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Cancel</span
                    >
                  </div>
                </nuxtlink>
              </div>
            </div>
            <div class="card-footer border-0" id="cardFooter"></div>
          </div>
        </div>

        <!-- Level 2 -->
        <div class="col-lg-3 col-md-6 mt-10">
          <div class="card card-stretch-50 shadow mb-5">
            <div
              class="position-absolute top-0 start-50 translate-middle d-flex justify-content-center rounded"
              id="bgBlue2"
            >
              <h1 class="mt-5" id="textBlue">2</h1>
            </div>
            <div class="card-body mt-10">
              <div class="text-center">
                <h2 id="textBlue" v-if="sales">
                  USD {{ formatPrice(sales.level2.total) }}
                </h2>
                <p class="text-muted">Attractive Proposal</p>
              </div>
              <div class="d-grid gap-2">
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level2Open()"
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span
                      id="btnGreen"
                      style="margin-left: -10px"
                      v-if="sales"
                      >{{ formatPrice(sales.level2.countOpen) }}</span
                    >
                    <span id="textGreen" v-if="sales"
                      >USD {{ formatPrice(sales.level2.open) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Open</span
                    >
                  </div>
                </nuxtlink>
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level2Cancel()"
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span id="btnRed" style="margin-left: -10px" v-if="sales">{{
                      formatPrice(sales.level2.countCancel)
                    }}</span>
                    <span id="textRed" v-if="sales"
                      >USD {{ formatPrice(sales.level2.cancel) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Cancel</span
                    >
                  </div>
                </nuxtlink>
              </div>
            </div>
            <div class="card-footer border-0" id="cardFooter"></div>
          </div>
        </div>

        <!-- Level 1 -->
        <div class="col-lg-3 col-md-6 mt-10">
          <div class="card card-stretch-50 shadow mb-5">
            <div
              class="position-absolute top-0 start-50 translate-middle d-flex justify-content-center rounded"
              id="bgGreen2"
            >
              <h1 class="mt-5" id="textGreen">1</h1>
            </div>
            <div class="card-body mt-10">
              <div class="text-center">
                <h2 id="textGreen" v-if="sales">
                  USD {{ formatPrice(sales.level1.total) }}
                </h2>
                <p class="text-muted">Contract Signing</p>
              </div>
              <div class="d-grid gap-2">
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level1Open()"
                  class="btn btn-outline btn-outline-dashed btn-outline-success d-flex justify-content-start btn-active-light-success me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span
                      id="btnGreen"
                      style="margin-left: -10px"
                      v-if="sales"
                      >{{ formatPrice(sales.level1.countOpen) }}</span
                    >
                    <span id="textGreen" v-if="sales"
                      >USD {{ formatPrice(sales.level1.open) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Open</span
                    >
                  </div>
                </nuxtlink>
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level1Cancel()"
                  class="btn btn-outline btn-outline-dashed btn-outline-danger d-flex justify-content-start btn-active-light-danger me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span id="btnRed" style="margin-left: -10px" v-if="sales">{{
                      formatPrice(sales.level1.countCancel)
                    }}</span>
                    <span id="textRed" v-if="sales"
                      >USD {{ formatPrice(sales.level1.cancel) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Cancel</span
                    >
                  </div>
                </nuxtlink>
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-warning d-flex justify-content-start btn-active-light-warning me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level1ClosedIn()"
                  class="btn btn-outline btn-outline-dashed btn-outline-warning d-flex justify-content-start btn-active-light-warning me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span
                      id="btnGold"
                      style="margin-left: -10px"
                      v-if="sales"
                      >{{ formatPrice(sales.level1.countClosedSales) }}</span
                    >
                    <span id="textGold" v-if="sales"
                      >USD {{ formatPrice(sales.level1.closedSales) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      style="margin-left: -15px"
                      id="fontSm"
                    >
                      Closed Sales
                    </span>
                  </div>
                </nuxtlink>
                <!-- <nuxtlink
                  class="btn btn-outline btn-outline-dashed btn-outline-info d-flex justify-content-start btn-active-light-info me-2 mb-2"
                  style="cursor: auto"
                > -->
                <nuxtlink
                  @click="level1Closed()"
                  class="btn btn-outline btn-outline-dashed btn-outline-info d-flex justify-content-start btn-active-light-info me-2 mb-2"
                  style="cursor: pointer"
                >
                  <div class="d-flex align-items-center gap-2">
                    <span
                      id="btnPurple"
                      style="margin-left: -10px"
                      v-if="sales"
                      >{{ formatPrice(sales.level1.countClosedIn) }}</span
                    >
                    <span id="textPurple" v-if="sales"
                      >USD {{ formatPrice(sales.level1.closedIn) }}</span
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-end">
                    <span
                      class="text-muted ms-10"
                      id="fontSm"
                      style="margin-left: -15px"
                      >Closed In</span
                    >
                  </div>
                </nuxtlink>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- End row 2 -->
    </div>

    <!-- Modal filter date  -->
    <div
      class="modal fade"
      id="filterdate"
      tabindex="-1"
      aria-labelledby="filterdateLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content modal-filter">
          <div class="modal-header text-center">
            <h1 class="modal-title w-100" id="filterdateLabel">Filter Date</h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <form>
            <div class="modal-body">
              <div class="row">
                <div class="col-lg-12">
                  <div class="mb-3">
                    <label class="form-label fw-bold">From</label>
                    <input
                      type="date"
                      class="form-control"
                      v-model="start_date"
                    />
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col-lg-12">
                  <div class="mb-3">
                    <label class="form-label fw-bold">Until</label>
                    <input
                      type="date"
                      class="form-control"
                      v-model="end_date"
                    />
                  </div>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              <div class="col-md-12 text-center">
                <button
                  type="button"
                  class="btn btn-light mx-3"
                  data-bs-dismiss="modal"
                  @click="closeModal()"
                >
                  Reset
                </button>
                <button
                  type="button"
                  class="btn btn-primary"
                  data-bs-dismiss="modal"
                  @click="list()"
                >
                  Filter
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
    <!-- End filter date -->

    <!-- Modal Upload -->
    <div
      class="modal fade"
      tabindex="-1"
      id="uploadFile"
      data-bs-backdrop="static"
    >
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h3 class="modal-title">Import Sales Data (Excel)</h3>

            <!--begin::Close-->
            <div
              ref="closeModalUpload"
              class="btn btn-icon btn-sm btn-active-light-primary ms-2"
              data-bs-dismiss="modal"
              aria-label="Close"
            >
              <span class="svg-icon svg-icon-1" @click="closeModalFile()">
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
            <!-- <button type="button" class="btn btn-info btn-block mb-8">
              <i class="fa-solid fa-download me-2"></i>Download Template (xlsx)
            </button> -->
            <form>
              <div class="form-group mb-3">
                <input
                  type="file"
                  @change="handleFileUpload"
                  id="fileUpload"
                  name="fileUpload"
                  ref="fileInput"
                  class="form-control"
                  :class="{
                    'is-invalid': errors.file,
                  }"
                />
              </div>
              <div class="row mt-10">
                <div class="col">
                  <button
                    type="button"
                    class="btn btn-info btn-block"
                    @click="downloadTemplate"
                  >
                    Download Template
                  </button>
                </div>
                <div class="col d-flex justify-content-end">
                  <button
                    type="button"
                    class="btn btn-secondary me-3"
                    data-bs-dismiss="modal"
                    id="close_modal_file"
                    @click="closeModalFile()"
                  >
                    Cancel
                  </button>
                  <button
                    type="button"
                    @click="confirmUpload"
                    class="btn btn-primary"
                  >
                    Upload
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <!-- End Modal Upload -->
  </div>
</template>

<script>
export default {
  layout: 'template',
  name: 'MySalesPlanPage',
  data() {
    return {
      getYear: new Date().getFullYear(),
      // getDate: moment(new Date()).format("YYYY/MM/DD"),
      getStart: new Date(new Date().getFullYear(), 0, 1),
      getEnd: new Date(new Date().getFullYear(), 11, 31),
      prospect_option_salesplan: [],
      aircraft_name_value: null,
      aircraft_id_value: null,
      product_id_value: null,
      prospect_value_salesplan: null,
      product_name_value: null,
      maintenance_name_value: null,
      maintenance_id_value: null,
      user: this.$auth.user.name,
      role: this.$auth.user.role.name,
      sales_table: {
        data: [],
        links: [],
      },
      p_sales: {
        id: null,
        customer: null,
        prospect: null,
        acReg: null,
        registration: null,
        other: null,
        type: null,
        level: null,
        progress: null,
        status: null,
      },
      search: null,

      product_option: [],
      customer_option: [],
      ams_option: [],

      ac_reg: null,
      is_rkap: null,
      value: null,
      tat: null,
      customer: null,
      ams: null,
      product: null,
      acReg: null,
      other: null,
      registration: null,
      transaction_type_id: null,
      type: null,
      level: null,
      progress: null,
      status: null,

      start_date: null,
      end_date: null,

      file: null,

      customer_id: null,
      customer_value: null,

      product_id: null,
      product_value: null,

      maintenance_id: null,
      maintenance_value: null,

      selectedYear: null,
      yearOptions: [],

      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: null,

      sales: null,
      sales_paginate: [],
      isDisabled: true,
      errors: {
        file: null,
        customer: null,
        prospect: null,
        acReg: null,
        registration: null,
        other: null,
        type: null,
        level: null,
        progress: null,
        status: null,
        customer_id: null,
        product_id: null,
        maintenance_id: null,
        transaction_type_id: null,
        hangar_id: null,
        ac_reg: null,
        value: null,
        tat: null,
        ac_type_id: null,
        prospect_id: null,
        start_date: null,
        end_date: null,
      },
    }
  },
  created() {
    this.list()
    this.listCustomer()
    this.listProduct()
    this.listAms()
  },
  mounted() {
    this.generateYearOptions()
  },
  methods: {
    resetFileInput() {
      this.$refs['fileInput'].value = ''
    },
    resetFilters() {
      this.selectedYear = null
      this.ams = null
      this.customer = null
      this.product = null
    },

    generateYearOptions() {
      const currentYear = new Date().getFullYear()

      for (let i = 0; i < 5; i++) {
        const year = currentYear - i
        this.yearOptions.push({
          value: year.toString(),
          text: year.toString(),
        })
      }
    },
    onYearSelect() {
      this.list()
    },

    handleMultiselectProduct(selectedItems) {
      const selectedProductIds = selectedItems.map((item) => item.id)

      this.selectedProducts = selectedProductIds

      this.list()
    },
    handleMultiselectCustomer(selectedItems) {
      const selectedCustomerIds = selectedItems.map((item) => item.id)

      this.selectedCustomers = selectedCustomerIds

      this.list()
    },
    handleMultiselectAms(selectedItems) {
      const selectedAmsInitials = selectedItems.map((item) => item.id)

      this.selectedAmsInitialName = selectedAmsInitials

      this.list()
    },

    handleFileUpload(event) {
      this.file = event.target.files[0]
    },

    downloadTemplate() {
      this.$axios
        .get('/api/template-excel', {
          responseType: 'blob',
        })
        .then((response) => {
          const tempLink = document.createElement('a')
          tempLink.href = window.URL.createObjectURL(new Blob([response.data]))
          tempLink.setAttribute('download', 'Template_Import_Sales.xlsx')
          document.body.appendChild(tempLink)
          tempLink.click()

          setTimeout(() => {
            document.body.removeChild(tempLink)
          }, 500)
        })
        .then(() => {
          toastr.success('Downloading Template Excel (Import Sales)')
        })
        .then((error) => console.log(error))
    },

    confirmUpload() {
      if (this.file) {
        Swal.fire({
          title: 'Upload Confirmation',
          text: 'Do you want to upload this file and import the data?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonText: 'Yes, upload it!',
          cancelButtonText: 'No, cancel!',
        }).then((result) => {
          if (result.isConfirmed) {
            this.uploadFile()
          }
        })
      } else {
        Swal.fire({
          icon: 'error',
          title: 'Please select a file first.',
          confirmButtonText: 'Got it',
        })
      }
    },
    uploadFile() {
      const formData = new FormData()
      formData.append('fileUpload', this.file)

      this.$refs.closeModalUpload.click()

      Swal.fire({
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
      })

      this.$axios
        .post('/api/sales-upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then((response) => {
          Swal.close()

          Swal.fire({
            icon: 'success',
            title: response.data.message,
            confirmButtonText: 'Close',
          })

          setTimeout(() => {
            this.resetFileInput()
            this.resetFilters()
            this.list()
          }, 1000)
        })
        .catch((error) => {
          Swal.close()

          Swal.fire({
            icon: 'error',
            title: error.response.data.message,
            confirmButtonText: 'Got it!',
          })

          this.resetFileInput()
        })
    },

    closeModal() {
      this.clearForm()
    },
    clearForm() {
      this.start_date = null
      this.end_date = null
      this.list()
    },

    closeModalFile() {
      this.clearFormFile()
      document.getElementById('uploadFile').classList.remove('show')
    },
    clearFormFile() {
      this.$refs.fileInput.value = ''
      this.file = null
      this.errors.file = null
    },

    formatPrice(value) {
      let val = (value / 1).toFixed(0).replace(',', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
    loading() {
      Swal.fire({
        timer: 1000,
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
      })
    },
    list() {
      this.loading()

      const selectedProducts =
        Array.isArray(this.selectedProducts) && this.selectedProducts.length
          ? this.selectedProducts
          : null
      const selectedCustomers =
        Array.isArray(this.selectedCustomers) && this.selectedCustomers.length
          ? this.selectedCustomers
          : null
      const selectedAmsInitialName =
        Array.isArray(this.selectedAmsInitialName) &&
        this.selectedAmsInitialName.length
          ? this.selectedAmsInitialName
          : null

      const params = {
        // tambahkan parameter query productId dengan nilai selectedProducts
        product: selectedProducts,
        customer: selectedCustomers,
        ams: selectedAmsInitialName,
        year: this.selectedYear ? this.selectedYear.value : null,
        start_date: this.start_date,
        end_date: this.end_date,
      }

      this.$axios
        .get('/api/sales-dashboard', { params })
        .then((response) => {
          this.sales = response.data.data
        })
        .catch((error) => console.log(error))
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
          this.customer_option = response.data.data.data
        })
    },
    listProduct() {
      this.$axios
        .get('api/product', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.product_option = response.data.data.data.map((product) => ({
            ...product,
            label: product.id,
          }))
        })
        .catch((error) => console.log(error))
    },
    listAms() {
      this.$axios
        .get('/api/ams', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.ams_option = response.data.data.data.map((ams) => ({
            ...ams,
            label: ams.initial,
          }))
        })
        .catch((error) => {
          console.error(error)
        })
    },

    filterTotal() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            other: '1',
            start_date: this.getStart,
            end_date: this.getEnd,
          },
        })
      })
    },
    filterOpen() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '1',
            start_date: this.getStart,
            end_date: this.getEnd,
            // year: this.getYear,
            // month: this.getDate,
          },
        })
      })
    },
    filterCancel() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '4',
            start_date: this.getStart,
            end_date: this.getEnd,
            // year: this.getYear,
          },
        })
      })
    },
    filterClosed() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '3',
            start_date: this.getStart,
            end_date: this.getEnd,
            // year: this.getYear,
          },
        })
      })
    },
    filterClosedIn() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '2',
            start_date: this.getStart,
            end_date: this.getEnd,
            // year: this.getYear,
          },
        })
      })
    },

    level4Open() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '1',
            level: '4',
          },
        })
      })
    },
    level4Cancel() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '4',
            level: '4',
          },
        })
      })
    },
    level3Open() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '1',
            level: '3',
          },
        })
      })
    },
    level3Cancel() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '4',
            level: '3',
          },
        })
      })
    },
    level2Open() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '1',
            level: '2',
          },
        })
      })
    },
    level2Cancel() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '4',
            level: '2',
          },
        })
      })
    },
    level1Open() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '1',
            level: '1',
          },
        })
      })
    },
    level1Cancel() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '4',
            level: '1',
          },
        })
      })
    },
    level1Closed() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '3',
            level: '1',
          },
        })
      })
    },
    level1ClosedIn() {
      this.$axios.get('api/sales-table').then((response) => {
        this.$router.push({
          name: 'my-salesplan-table',
          params: {
            status: '2',
            level: '1',
          },
        })
      })
    },
  },
}
</script>

<style>
@media (min-width: 320px) {
  #cardStyle {
    width: auto;
  }
}

@media (min-width: 1281px) {
  #cardStyle {
    margin-left: -15px;
    width: 258px;
  }

  #cardFooter {
    height: 134px;
  }
}

.mt-20 {
  margin-top: 20px;
}

.mb-20 {
  margin-bottom: 20px;
}

.carousel-indicators [data-bs-target] {
  box-sizing: content-box;
  flex: 0 1 auto;
  width: 10px;
  height: 10px;
  padding: 0;
  margin-right: 3px;
  margin-left: 3px;
  text-indent: -999px;
  cursor: pointer;
  background-color: #000;
  background-clip: padding-box;
  border: 0;
  border-top: 10px solid transparent;
  border-bottom: 10px solid transparent;
  opacity: 0.5;
  transition: opacity 0.6s ease;
  border-radius: 100%;
}

.carousel-indicators .active {
  background-color: #188af8;
}

#cursorPinter {
  cursor: pointer;
}

#btnGreen {
  background-color: #dff0d0;
  color: #5e932f;
  border-radius: 100%;
  padding: 5px 13px 5px 13px;
}

#btnRed {
  background-color: #f8e8e8;
  color: #952d2d;
  border-radius: 100%;
  padding: 5px 13px 5px 13px;
}

#btnPurple {
  background-color: #f8e8f6;
  color: #952d88;
  border-radius: 100%;
  padding: 5px 13px 5px 13px;
}

#btnGold {
  background-color: #f8f7e8;
  color: #958e2d;
  border-radius: 100%;
  padding: 5px 13px 5px 13px;
}

#bgGreen {
  background: #dff0d0;
}

#bgBlue {
  background: #e8e9f8;
}

#bgRed {
  background: #f8e8e8;
}

#bgPurple {
  background: #f8e8f6;
}

#bgGold {
  background: #f8f7e8;
}

#bgGreen2 {
  background-color: #dff0d0;
  width: 60px;
  height: 60px;
}

#bgBlue2 {
  background-color: #d0d3f1;
  width: 60px;
  height: 60px;
}

#bgPurple2 {
  background-color: #f1d0ee;
  width: 60px;
  height: 60px;
}

#bgGold2 {
  background-color: #f1efd0;
  width: 60px;
  height: 60px;
}

#textGreen {
  color: #5e932f;
}

#textRed {
  color: #952d2d;
}

#textBlue {
  color: #2d3495;
}

#textPurple {
  color: #952d88;
}

#textGold {
  color: #958e2d;
}

#fontSm {
  font-size: 12px;
}
</style>
