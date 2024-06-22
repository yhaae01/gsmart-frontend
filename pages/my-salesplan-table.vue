<template>
  <div>
    <div class="container-fluid mt-10 mb-20">
      <div class="row">
        <div class="col-lg-6">
          <Nuxt-link class="menu-link" to="/my-salesplan" active-class="active">
            <i class="fa-solid fa-angle-left"></i>
            <span
              class="menu-title"
              v-if="role == 'TPR' || role == 'CBO' || role == 'Administrator'"
              >Sales Plan</span
            >
            <span class="menu-title" v-if="role == 'AMS' || role == 'AM'"
              >Sales Plan</span
            >
          </Nuxt-link>
        </div>
      </div>
      <div class="mt-5">
        <h5>Total Value: USD {{ formatPrice(totalValue) }}</h5>
      </div>
      <!-- Head Salesplan Table -->
      <div class="row">
        <div class="col-lg-6 col-sm-12">
          <h3 class="mt-5">Sales Plan Table</h3>
        </div>
        <div class="col-lg-6 col-sm-12 d-flex justify-content-end">
          <button
            type="button"
            class="btn btn-info btn-sm my-3 me-4"
            data-bs-toggle="modal"
            data-bs-target="#filterdate"
          >
            <i class="fa-solid fa-calendar-days"></i>
            Filter Date
          </button>
          <button
            class="btn btn-primary btn-sm my-3"
            data-bs-toggle="modal"
            data-bs-target="#addSales"
            v-if="role === 'AMS' || role === 'AM'"
          >
            <i class="fas fa-plus"></i> Additional Sales Plan
          </button>
        </div>
      </div>

      <!-- Modal filter data  -->
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
              <h1 class="modal-title w-100" id="filterdateLabel">
                Filter Date
              </h1>
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
                    @click="listTable()"
                  >
                    Filter
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>

      <!-- Modal addSales  -->
      <div
        class="modal fade"
        tabindex="-1"
        id="addSales"
        data-bs-backdrop="static"
      >
        <div class="modal-dialog modal-lg modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h3>Additional Sales Plan</h3>

              <!--begin::Close-->
              <div
                class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                data-bs-dismiss="modal"
                aria-label="Close"
              >
                <span class="svg-icon svg-icon-1" @click="closeAddSales()">
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
              <div class="row">
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Customer</label>
                    <multiselect
                      v-model="additional.customer_id"
                      :options="customer_option"
                      open-direction="bottom"
                      placeholder="Select Customer"
                      label="label"
                      :searchable="true"
                      :class="{ 'is-invalid': errors.customer_id }"
                    ></multiselect>
                    <span
                      v-if="errors.customer_id"
                      class="error invalid-feedback"
                      >{{ errors.customer_id[0] }}</span
                    >
                  </div>
                </div>
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Aircraft Type</label>
                    <multiselect
                      v-model="additional.ac_type_id"
                      :options="ac_type_option"
                      open-direction="bottom"
                      placeholder="Select Aircraft Type"
                      label="name"
                      :searchable="true"
                      :class="{ 'is-invalid': errors.ac_type_id }"
                    ></multiselect>
                    <span
                      v-if="errors.ac_type_id"
                      class="error invalid-feedback"
                      >{{ errors.ac_type_id[0] }}</span
                    >
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Product</label>
                    <multiselect
                      v-model="additional.product_id"
                      :options="product_option"
                      open-direction="bottom"
                      placeholder="Select Product"
                      label="name"
                      :searchable="true"
                      @input="listMaintenance()"
                      :class="{ 'is-invalid': errors.product_id }"
                    ></multiselect>
                    <span
                      v-if="errors.product_id"
                      class="error invalid-feedback"
                      >{{ errors.product_id[0] }}</span
                    >
                  </div>
                </div>
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Maintenance</label>
                    <multiselect
                      v-model="additional.maintenance_id"
                      :options="maintenance_option"
                      :multiple="true"
                      :searchable="true"
                      :close-on-select="false"
                      :clear-on-select="false"
                      track-by="id"
                      label="name"
                      placeholder="Select Maintenance"
                      :disabled="!additional.product_id"
                      :class="{ 'is-invalid': errors.maintenance_id }"
                    ></multiselect>
                    <span
                      v-if="errors.maintenance_id"
                      class="error invalid-feedback"
                      >{{ errors.maintenance_id[0] }}</span
                    >
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Registration/SN/PN</label>
                    <input
                      required
                      type="text"
                      v-model="additional.ac_reg"
                      class="form-control"
                      :class="{ 'is-invalid': errors.ac_reg }"
                    />
                    <span v-if="errors.ac_reg" class="error invalid-feedback">{{
                      errors.ac_reg[0]
                    }}</span>
                  </div>
                </div>
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Sales Plan</label>
                    <input
                      type="number"
                      v-model="additional.value"
                      class="form-control"
                      :class="{ 'is-invalid': errors.value }"
                    />
                    <span v-if="errors.value" class="error invalid-feedback">{{
                      errors.value[0]
                    }}</span>
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Start Date</label>
                    <input
                      required
                      type="date"
                      v-model="additional.start_date"
                      class="form-control"
                      :class="{ 'is-invalid': errors.start_date }"
                    />
                    <span
                      v-if="errors.start_date"
                      class="error invalid-feedback"
                      >{{ errors.start_date[0] }}</span
                    >
                  </div>
                </div>
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">TAT</label>
                    <input
                      required
                      type="number"
                      v-model="additional.tat"
                      class="form-control"
                      :class="{ 'is-invalid': errors.tat }"
                    />
                    <span v-if="errors.tat" class="error invalid-feedback">{{
                      errors.tat[0]
                    }}</span>
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-lg-6">
                  <div class="form-group mb-3">
                    <label class="form-label fw-bold">Type</label>
                    <select
                      v-model="additional.transaction_type_id"
                      class="form-select"
                      :class="{ 'is-invalid': errors.transaction_type_id }"
                    >
                      <option :value="null">Select Type</option>
                      <option value="1">TMB Retail</option>
                      <option value="2">TMB Project</option>
                    </select>
                    <!-- <multiselect
                      v-model="transaction_type_id"
                      :options="transaction_type_option"
                      placeholder="Select Type"
                      label="name"
                    ></multiselect> -->
                    <span
                      v-if="errors.transaction_type_id"
                      class="error invalid-feedback"
                      >{{ errors.transaction_type_id[0] }}</span
                    >
                  </div>
                </div>
              </div>

              <div class="row mt-10">
                <div class="col">
                  <button
                    type="button"
                    class="btn btn-light"
                    data-bs-dismiss="modal"
                    @click="closeAddSales()"
                    id="close_modal"
                  >
                    Back
                  </button>
                </div>
                <div class="col d-flex justify-content-end">
                  <button
                    type="button"
                    @click="addSales()"
                    class="btn btn-primary"
                  >
                    Save
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Row Table -->
      <div class="row mt-3">
        <div class="card">
          <div class="card-body">
            <!-- Search -->
            <div class="row d-flex align-items-center mb-5">
              <div class="col-11">
                <div class="row">
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
                  <input
                    type="text"
                    class="form-control form-control-solid ps-10"
                    name="search"
                    v-model="search"
                    placeholder="Search"
                  />
                </div>
              </div>
              <div class="col">
                <button class="btn btn-danger btn-sm" @click="removeSearch()">
                  Clear
                </button>
              </div>
            </div>
            <!-- Table -->
            <div class="table-responsive">
              <table class="table table-row-bordered table-row-gray-200 gy-4">
                <thead>
                  <tr class="fw-bold fs-6 text-gray-800 table-secondary">
                    <th class="text-center"><p>No</p></th>
                    <!-- Start area sorting -->
                    <th class="text-center" style="white-space: nowrap">
                      <p>AREA</p>
                    </th>
                    <!-- End area sorting -->

                    <!-- Start ams sorting -->
                    <span v-if="role != 'AMS'">
                      <th
                        v-if="order == 'ams' && by == 'asc'"
                        @click="sort('ams', 'desc')"
                        class="text-center"
                        style="white-space: nowrap; cursor: pointer"
                      >
                        <p class="d-flex justify-content-center">
                          AMS
                          <i
                            class="fa-solid fa-sort-up ms-2"
                            style="color: black"
                          ></i>
                        </p>
                      </th>
                      <th
                        v-else-if="order == 'ams' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap; cursor: pointer"
                      >
                        <p class="d-flex justify-content-center">
                          AMS
                          <i
                            class="fa-solid fa-sort-down ms-2"
                            style="color: black"
                          ></i>
                        </p>
                      </th>
                      <th
                        v-else
                        @click="sort('ams', 'asc')"
                        class="text-center"
                        style="white-space: nowrap; cursor: pointer"
                      >
                        <p class="d-flex justify-content-center">
                          AMS
                          <i class="fa-solid fa-sort ms-2"></i>
                        </p>
                      </th>
                    </span>
                    <!-- End ams sorting -->

                    <!-- Start customer sorting -->
                    <th
                      v-if="order == 'customer' && by == 'asc'"
                      @click="sort('customer', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        CUSTOMER
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'customer' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        CUSTOMER
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('customer', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        CUSTOMER
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End customer sorting -->

                    <!-- Start product sorting -->
                    <th
                      v-if="order == 'product' && by == 'asc'"
                      @click="sort('product', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        PRODUCT
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'product' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        PRODUCT
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('product', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        PRODUCT
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End product sorting -->

                    <!-- Start registration sorting -->
                    <th
                      v-if="order == 'registration' && by == 'asc'"
                      @click="sort('registration', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        AC/ENG/APU/COMP
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'registration' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        AC/ENG/APU/COMP
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('registration', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        AC/ENG/APU/COMP
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End registration sorting -->

                    <!-- Start acReg sorting -->
                    <th
                      v-if="order == 'acReg' && by == 'asc'"
                      @click="sort('acReg', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        REGISTRATION
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'acReg' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        REGISTRATION
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('acReg', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        REGISTRATION
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End acReg sorting -->

                    <!-- Start other sorting -->
                    <th
                      v-if="order == 'other' && by == 'asc'"
                      @click="sort('other', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Sales Type
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'other' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Sales Type
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('other', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Sales Type
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End other sorting -->

                    <!-- Start type sorting -->
                    <th
                      v-if="order == 'type' && by == 'asc'"
                      @click="sort('type', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Type
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'type' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Type
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('type', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Type
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End type sorting -->

                    <!-- Start level sorting -->
                    <th
                      v-if="order == 'level' && by == 'asc'"
                      @click="sort('level', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Level
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'level' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Level
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('level', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Level
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End level sorting -->

                    <!-- Start progress sorting -->
                    <th
                      v-if="order == 'progress' && by == 'asc'"
                      @click="sort('progress', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Progress
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'progress' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Progress
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('progress', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        Progress
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End progress sorting -->

                    <!-- Start status sorting -->
                    <th
                      v-if="order == 'status' && by == 'asc'"
                      @click="sort('status', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        STATUS
                        <i
                          class="fa-solid fa-sort-up ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else-if="order == 'status' && by == 'desc'"
                      @click="sort('id', 'desc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        STATUS
                        <i
                          class="fa-solid fa-sort-down ms-2"
                          style="color: black"
                        ></i>
                      </p>
                    </th>
                    <th
                      v-else
                      @click="sort('status', 'asc')"
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p class="d-flex justify-content-center">
                        STATUS
                        <i class="fa-solid fa-sort ms-2"></i>
                      </p>
                    </th>
                    <!-- End progress sorting -->
                    <th
                      class="text-center"
                      style="white-space: nowrap; cursor: pointer"
                    >
                      <p>ACTION</p>
                    </th>
                  </tr>
                </thead>
                <tr class="text-center">
                  <td></td>
                  <!-- Select Area -->
                  <td>
                    <multiselect
                      v-if="role != 'AMS'"
                      v-model="area"
                      :options="area_option"
                      open-direction="bottom"
                      placeholder=""
                      label="label"
                      :multiple="true"
                      :close-on-select="false"
                      :clear-on-select="false"
                      :preserve-search="true"
                      track-by="name"
                      :preselect-first="true"
                      :class="{ 'is-invalid': errors.area }"
                      @input="handleMultiselectArea"
                    ></multiselect>
                  </td>
                  <!-- Select AMS -->
                  <td>
                    <multiselect
                      v-if="role != 'AMS'"
                      v-model="ams"
                      :options="ams_option"
                      open-direction="bottom"
                      placeholder=""
                      label="label"
                      :multiple="true"
                      :close-on-select="false"
                      :clear-on-select="false"
                      :preserve-search="true"
                      track-by="initial"
                      :preselect-first="true"
                      :class="{ 'is-invalid': errors.ams }"
                      @input="handleMultiselectAms"
                    ></multiselect>
                  </td>
                  <!-- Select Customer -->
                  <td>
                    <multiselect
                      v-model="customer"
                      :options="customer_option"
                      open-direction="bottom"
                      placeholder=""
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
                  </td>
                  <!-- Select Product -->
                  <td>
                    <multiselect
                      v-model="product"
                      :options="product_option"
                      open-direction="bottom"
                      placeholder=""
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
                  </td>
                  <td></td>
                  <!-- Select ACReg -->
                  <td>
                    <multiselect
                      v-model="acReg"
                      :options="transformRegistrationOptions()"
                      open-direction="bottom"
                      placeholder=""
                      label="label"
                      :multiple="true"
                      :close-on-select="false"
                      :clear-on-select="false"
                      :preserve-search="true"
                      track-by="label"
                      :preselect-first="true"
                      :class="{ 'is-invalid': errors.acReg }"
                      @input="handleMultiselectAcReg"
                    ></multiselect>
                  </td>
                  <!-- Select Other -->
                  <td>
                    <multiselect
                      v-model="other"
                      :options="otherOptions"
                      :multiple="true"
                      track-by="value"
                      label="label"
                      placeholder=""
                      @click="listTable()"
                    ></multiselect>
                  </td>
                  <!-- Select Type -->
                  <td>
                    <multiselect
                      v-model="type"
                      :options="typeOptions"
                      :multiple="true"
                      track-by="value"
                      label="label"
                      placeholder=""
                      @click="listTable()"
                    ></multiselect>
                  </td>
                  <!-- Select Level -->
                  <td>
                    <multiselect
                      v-model="level"
                      :options="levelOptions"
                      :multiple="true"
                      track-by="value"
                      label="label"
                      placeholder=""
                      @click="listTable()"
                    ></multiselect>
                  </td>
                  <!-- Select Progress -->
                  <td>
                    <input
                      type="number"
                      class="form-control ms-10"
                      v-model="progress"
                      style="width: 80px"
                    />
                    <!-- <multiselect
                      v-model="progress"
                      :options="progressOptions"
                      :multiple="true"
                      track-by="value"
                      placeholder=""
                      :taggable="true"
                      @tag="handleTagProgress"
                      @remove="handleRemoveProgress"
                    ></multiselect> -->
                  </td>
                  <!-- Select Status -->
                  <td>
                    <multiselect
                      v-model="status"
                      :options="statusOptions"
                      :multiple="true"
                      track-by="value"
                      label="label"
                      placeholder=""
                      @click="listTable()"
                    ></multiselect>
                  </td>
                  <td>
                    <!-- <button
                      type="button"
                      class="btn btn-success ms-2"
                      @click="closeModal()"
                      data-bs-toggle="tooltip"
                      data-bs-placement="top"
                      title="Reset Filter"
                    >
                      Reset
                    </button> -->
                  </td>
                </tr>
                <tbody>
                  <tr
                    v-for="(p_sales, p_sales_index) in sales_table.data"
                    :key="p_sales_index"
                  >
                    <td class="text-center">
                      {{ sales_paginate.from + p_sales_index }}.
                    </td>
                    <!-- Area -->
                    <td class="text-center">
                      {{ p_sales.area }}
                    </td>
                    <!-- AMS -->
                    <td v-if="role != 'AMS'" class="text-center">
                      {{ p_sales.ams }}
                    </td>
                    <!-- Customer -->
                    <td class="text-center">
                      {{ p_sales.customer }}
                    </td>
                    <!-- Product -->
                    <td class="text-center" v-if="p_sales.type == 'PBTH'">
                      {{ p_sales.product }} - {{ p_sales.month }}
                    </td>
                    <td class="text-center" v-else>
                      {{ p_sales.product }}
                    </td>
                    <!-- AC/ENG/APU/COMP -->
                    <td class="text-center">
                      {{ p_sales.registration }}
                    </td>
                    <!-- REGISTRATION -->
                    <td class="text-center">
                      {{ p_sales.acReg }}
                    </td>
                    <!-- Other -->
                    <td class="text-center">
                      <b>{{ p_sales.other }}</b>
                    </td>
                    <!-- Type -->
                    <td class="text-center">
                      <b style="white-space: nowrap">{{ p_sales.type }}</b>
                    </td>
                    <!-- Sales Level -->
                    <td class="text-center">
                      <div
                        :class="{
                          'badge badge-success': p_sales.level === 1,
                          'badge badge-warning': p_sales.level === 2,
                          'badge badge-primary': p_sales.level === 3,
                          'badge badge-danger': p_sales.level === 4,
                        }"
                      >
                        Level {{ p_sales.level }}
                      </div>
                    </td>
                    <!-- Progress -->
                    <td class="text-center">
                      <div
                        v-if="p_sales.progress >= 1 && p_sales.progress <= 30"
                      >
                        <span class="badge badge-danger"
                          >{{ p_sales.progress }}%</span
                        >
                      </div>
                      <div
                        v-if="p_sales.progress >= 31 && p_sales.progress <= 60"
                      >
                        <span class="badge badge-warning"
                          >{{ p_sales.progress }}%</span
                        >
                      </div>
                      <div
                        v-if="p_sales.progress >= 61 && p_sales.progress <= 80"
                      >
                        <span class="badge badge-primary"
                          >{{ p_sales.progress }}%</span
                        >
                      </div>
                      <div
                        v-if="p_sales.progress >= 81 && p_sales.progress <= 100"
                      >
                        <span class="badge badge-success"
                          >{{ p_sales.progress }}%</span
                        >
                      </div>
                    </td>
                    <!-- Status -->
                    <td class="text-center">
                      <div v-if="p_sales.request">
                        <span
                          v-if="p_sales.request.reviewer_id == user_id"
                          class="badge badge-warning text-dark"
                        >
                          Need Your Approval
                          <span
                            class="spinner-grow spinner-grow-sm ms-1"
                            role="status"
                            aria-hidden="true"
                          ></span>
                        </span>
                        <span v-else class="badge badge-info">
                          Waiting Approval
                        </span>
                      </div>
                      <div v-else>
                        <div v-if="p_sales.status === 'Cancel'">
                          <span class="badge badge-danger">{{
                            p_sales.status
                          }}</span>
                        </div>
                        <div v-if="p_sales.status === 'Open'">
                          <span class="badge badge-gmf">{{
                            p_sales.status
                          }}</span>
                        </div>
                        <div v-if="p_sales.status === 'Closed Sales'">
                          <span class="badge badge-green">
                            {{ p_sales.status }}
                          </span>
                        </div>
                        <div v-if="p_sales.status === 'Closed In'">
                          <span class="badge badge-success">{{
                            p_sales.status
                          }}</span>
                        </div>
                      </div>
                    </td>
                    <td class="text-center">
                      <nuxt-link
                        v-if="p_sales"
                        :to="{
                          path: '/my-salesplan-detail',
                          query: { id: p_sales.id },
                        }"
                      >
                        <span class="menu-title">Detail</span>
                      </nuxt-link>
                    </td>
                  </tr>
                  <!-- Jika data kosong -->
                  <tr v-if="sales_table.data.length < 1">
                    <td colspan="12">
                      <div class="text-muted text-center">Data not found</div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- Pagination -->
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
                        @change="listTable()"
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
                <nav aria-label="Page navigation example">
                  <ul class="pagination">
                    <!-- Start pagination -->
                    <li
                      v-for="(link, link_index) in sales_table.links"
                      :key="link_index"
                      class="page-item"
                      :class="{ disabled: !link.url, active: link.active }"
                    >
                      <a
                        href="javascript:void(0)"
                        @click="onPageClick(link.url)"
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
      <!-- End Table -->
    </div>
  </div>
</template>

<script>
import debounce from 'lodash/debounce'
export default {
  layout: 'template',
  name: 'MySalesPlanTablePage',
  data() {
    return {
      selectedLevel: null,
      selectedStatus: null,

      selectedArea: [],
      prospect_option_salesplan: [],
      aircraft_name_value: null,
      aircraft_id_value: null,
      product_id_value: null,
      prospect_value_salesplan: null,
      product_name_value: null,
      maintenance_name_value: null,
      maintenance_id_value: null,
      user: this.$auth.user.name,
      user_id: this.$auth.user.id,
      role: this.$auth.user.role.name,

      sales_table: {
        data: [],
        links: [],
      },
      p_sales: {
        id: null,
        area: null,
        ams: null,
        customer: null,
        prospect: null,
        acReg: null,
        registration: null,
        other: null,
        type: null,
        level: null,
        progress: null,
        status: null,
        request: null,
      },
      search: this.$cookies.get('search'),

      additional: {
        customer_id: null,
        product_id: null,
        maintenance_id: [],
        ac_type_id: null,
        ac_reg: null,
        value: null,
        start_date: null,
        tat: null,
        transaction_type_id: null,
      },

      area_option: [],
      area: null,

      ams_option: [],
      ams: null,

      customer_option: [],
      customer: null,

      product_option: [],
      product: null,

      registration_option: [],
      acReg: null,

      ac_type_option: [],
      ac_type_id: null,

      maintenance_option: [],
      maintenance: null,

      other: [],
      otherOptions: [
        { value: 1, label: 'RKAP' },
        { value: 0, label: 'Additional' },
      ],

      type: [],
      typeOptions: [
        { value: 1, label: 'TMB Retail' },
        { value: 2, label: 'TMB Project' },
        { value: 3, label: 'PBTH' },
      ],

      level: [],
      levelOptions: [
        { value: 1, label: 'Level 1' },
        { value: 2, label: 'Level 2' },
        { value: 3, label: 'Level 3' },
        { value: 4, label: 'Level 4' },
      ],

      progress: null,
      // progress: [],
      progressOptions: [],

      status: [],
      statusOptions: [
        { value: 1, label: 'Open' },
        { value: 2, label: 'Closed Sales' },
        { value: 3, label: 'Closed In' },
        { value: 4, label: 'Cancel' },
      ],

      component_option: null,
      apu_option: null,
      engine_option: null,

      totalValue: null,

      ac_reg: null,
      is_rkap: null,
      value: null,
      tat: null,
      registration: null,
      transaction_type_id: null,
      ac_type_value: null,

      customer_id: null,
      customer_value: null,

      product_id: null,
      product_value: null,

      maintenance_id: null,
      maintenance_value: null,

      hangar_id: null,
      hangar_value: null,

      order: 'id',
      by: 'desc',
      paginate: '10',
      current_page: 1,
      totalPages: 10,

      areaLabel: null,
      amsLabel: null,
      customerLabel: null,
      productLabel: null,
      registrationLabel: null,
      selectedCustomers: [],
      acReg: [],

      sales: null,
      sales_paginate: [],
      isDisabled: true,
      areaChanged: false,
      errors: {
        area: null,
        ams: null,
        customer: null,
        prospect: null,
        acReg: null,
        registration: null,
        other: null,
        type: null,
        level: null,
        progress: null,
        remarks: null,
        status: null,
        customer_id: null,
        product: null,
        maintenance_id: null,
        transaction_type_id: null,
        hangar_id: null,
        ac_reg: null,
        value: null,
        tat: null,
        ac_type_id: null,
        prospect_id: null,
        start_date: null,
      },
    }
  },
  watch: {
    area: {
      deep: true,
      handler: function (newVal, oldVal) {
        // Periksa apakah ada perubahan pada area
        if (JSON.stringify(newVal) !== JSON.stringify(oldVal)) {
          this.listTable() // Panggil listTable saat area berubah
        }
      },
    },
    search: debounce(function () {
      this.listTable()
    }, 800),
    ams: debounce(function () {
      this.listTable()
    }, 500),
    area: debounce(function () {
      this.listTable()
    }, 500),
    customer: debounce(function () {
      this.listTable()
    }, 500),
    product: debounce(function () {
      this.listTable()
    }, 500),
    acReg: debounce(function () {
      this.listTable()
    }, 500),
    component_id: debounce(function () {
      this.listTable()
    }, 500),
    apu_id: debounce(function () {
      this.listTable()
    }, 500),
    engine_id: debounce(function () {
      this.listTable()
    }, 500),
    other: debounce(function () {
      this.listTable()
    }, 500),
    type: debounce(function () {
      this.listTable()
    }, 500),
    progress: debounce(function () {
      this.listTable()
    }, 500),
    status: debounce(function () {
      this.listTable()
    }, 500),
    level: debounce(function () {
      this.listTable()
    }, 500),
  },
  created() {
    this.getFilter()
    this.listTable()
    this.listCustomer()
    this.listProduct()
    this.listMaintenance()
    this.listACType()
    this.listComponent()
    this.listApu()
    this.listEngine()
    this.listRegistration()
    this.listArea()
    this.listAms()
  },
  mounted() {
    // Set this.level from the route parameter when component is mounted
    const level = this.$route.params.level
    const status = this.$route.params.status

    this.selectedLevel = level
    this.selectedStatus = status
    // Call listTable() here if needed
    this.listTable()
  },
  methods: {
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.sales_paginate.last_page) {
        this.current_page = this.sales_paginate.last_page
      }
      let url = new URL(this.sales_paginate.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.listTable(new_url)
    }, 500),
    handleMultiselectArea(selectedItems) {
      const selectedNames = selectedItems.map((item) => item.name)
      const encodedNames = selectedNames.map((name) =>
        name.includes(',') ? encodeURIComponent(name) : name
      )

      // Set the areaLabel property
      this.areaLabel = encodedNames.join(',')

      // Update the query parameters
      this.listTable()
    },
    handleMultiselectAms(selectedItems) {
      const selectedInitials = selectedItems.map((item) => item.initial)
      const encodedInitials = selectedInitials.map((initial) =>
        initial.includes(',') ? encodeURIComponent(initial) : initial
      )

      // Set the amsLabel property
      this.amsLabel = encodedInitials.join(',')

      // Update the query parameters
      this.listTable()
    },
    handleMultiselectCustomer(selectedItems) {
      // Extract the 'id' property from selectedItems
      const selectedCustomerIds = selectedItems.map((item) => item.id)

      // Update the selectedCustomers array with the extracted ids
      this.selectedCustomers = selectedCustomerIds

      // Update the query parameters
      this.listTable()
    },
    handleMultiselectProduct(selectedItems) {
      // Extract the 'id' property from selectedItems
      const selectedProductIds = selectedItems.map((item) => item.id)

      // Update the selectedProducts array with the extracted ids
      this.selectedProducts = selectedProductIds

      // Update the query parameters
      this.listTable()
    },
    handleMultiselectAcReg(selectedItems) {
      // Update the selectedAcReg array with the selectedItems
      this.selectedAcReg = selectedItems

      // Update the query parameters
      this.listTable()
    },
    handleTagProgress(newTag) {
      this.progress.push(newTag)
    },
    handleRemoveProgress(itemToRemove) {
      // Temukan indeks item yang akan dihapus
      const indexToRemove = this.progress.findIndex(
        (item) => item.value === itemToRemove.value
      )

      // Hapus item dari array progress
      if (indexToRemove !== -1) {
        this.progress.splice(indexToRemove, 1)
      }
    },

    onPageClick(url) {
      if (url) {
        // Extract the page number from the URL using your logic
        const match = url.match(/page=(\d+)/)
        const pageNumber = match ? parseInt(match[1]) : 1

        // Update the current_page value
        this.current_page = pageNumber

        // Call your method to fetch data for the new page
        this.listTable()
      }
    },

    sort(order, by) {
      this.order = order
      this.by = by
      this.listTable()
    },
    formatPrice(value) {
      // Tidak dibulatkan
      if (value === null || value === undefined) {
        return ''
      }
      return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')

      // Dibulatkan
      // let val = (value / 1).toFixed(0).replace(',', ',')
      // return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
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
    // Filter dari halaman dashboard sales
    getFilter() {
      this.loading()
      this.status = this.$route.params.status
      this.level = this.$route.params.level
      this.start_date = this.$route.params.start_date
      this.end_date = this.$route.params.end_date
      this.other = this.$route.params.other
    },
    listTable() {
      this.loading()
      const paginate = '/api/sales-table'

      const selectedAreaNames = this.area
        ? this.area.map((item) => item.name)
        : []
      const selectedInitials = this.ams
        ? this.ams.map((item) => item.initial)
        : []
      const selectedCustomers =
        Array.isArray(this.selectedCustomers) && this.selectedCustomers.length
          ? this.selectedCustomers
          : null
      const selectedProducts =
        Array.isArray(this.selectedProducts) && this.selectedProducts.length
          ? this.selectedProducts
          : null
      const selectedAcReg =
        Array.isArray(this.selectedAcReg) && this.selectedAcReg.length
          ? this.selectedAcReg.map((item) => item.label)
          : null
      const selectedOther = this.other
        ? this.other.map((item) => item.value)
        : []
      const selectedType = this.type ? this.type.map((item) => item.value) : []
      // const selectedLevel =
      //   Array.isArray(this.level) && this.level.length
      //     ? this.level.map((item) => item.value)
      //     : []
      // const selectedStatus =
      //   Array.isArray(this.status) && this.status.length
      //     ? this.status.map((item) => item.value)
      //     : []

      const selectedLevel = this.selectedLevel ? [this.selectedLevel] : []
      const selectedStatus = this.selectedStatus ? [this.selectedStatus] : []

      const selectedProgress = this.progress

      const params = {
        search: this.search,
        order: this.order,
        by: this.by,
        paginate: this.paginate,
        page: this.current_page,
        start_date: this.start_date,
        end_date: this.end_date,
        product: selectedProducts,
        ams: selectedInitials,
        customer: selectedCustomers,
        area: selectedAreaNames,
        acReg: selectedAcReg,
        ac_type_id: this.ac_type_id,
        component_id: this.component_id,
        apu_id: this.apu_id,
        engine_id: this.engine_id,
        other: selectedOther,
        type: selectedType,
        progress: selectedProgress,
        status: selectedStatus,
        level: selectedLevel,
      }

      // Tambahkan parameter level[] hanya jika this.level sudah terdefinisi
      if (Array.isArray(this.level)) {
        params.level = this.level.map((item) => item.value)
      }

      this.$axios
        .get(paginate, { params })
        .then((response) => {
          this.sales_table.data = response.data.data.data
          this.totalValue = response.data.totalValue
          this.sales_table.links = response.data.data.links
          this.sales_paginate = response.data.data
          this.current_page = this.sales_paginate.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    removeSearch() {
      this.clearForm()
      this.search = null
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
    listMaintenance() {
      if (!this.additional.product_id) {
        this.maintenance_option = []
        return
      }
      const productId = this.additional.product_id.id
      this.$axios
        .get('api/maintenance', {
          params: {
            order: 'created_at',
            by: 'ASC',
            product_id: productId,
          },
        })
        .then((response) => {
          this.maintenance_option = response.data.data.data.map(
            (maintenance) => ({
              ...maintenance,
              label: maintenance.name,
            })
          )
        })
        .catch((error) => console.log(error))
    },
    listACType() {
      this.$axios
        .get('/api/aircraft-type', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.ac_type_option = response.data.data.data.map((ac_type_id) => ({
            ...ac_type_id,
            label: ac_type_id.id,
          }))
        })
        .catch((error) => {
          console.log(error)
        })
    },
    listComponent() {
      this.$axios
        .get('/api/component', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.component_option = response.data.data.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    listApu() {
      this.$axios
        .get('/api/apu', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.apu_option = response.data.data.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    listEngine() {
      this.$axios
        .get('/api/engine', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.engine_option = response.data.data.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    listRegistration() {
      this.$axios
        .get('/api/sales-acreg')
        .then((response) => {
          this.registration_option = response.data.data
        })
        .catch((error) => {
          console.error('Error fetching data from /api/sales-acreg:', error)
        })
    },
    // Untuk membaca list registrasi
    transformRegistrationOptions() {
      return this.registration_option.map((item) => ({
        label: item, // assuming 'label' is the property you want to use
      }))
    },
    listArea() {
      this.$axios
        .get('/api/area', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.area_option = response.data.data.data.map((area) => {
            return {
              ...area,
              label: area.name,
            }
          })
        })
        .catch((error) => {
          console.error(error)
        })
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

    addSales() {
      // Reset error messages
      this.errors = {}

      // Validate required fields
      const requiredFields = {
        customer_id: this.additional.customer_id,
        product_id: this.additional.product_id,
        ac_reg: this.additional.ac_reg,
        value: this.additional.value,
        start_date: this.additional.start_date,
        tat: this.additional.tat,
        transaction_type_id: this.additional.transaction_type_id,
      }

      let hasError = false

      for (const [key, value] of Object.entries(requiredFields)) {
        if (!value || (Array.isArray(value) && value.length === 0)) {
          this.errors[key] = [`${key.replace(/_/g, ' ')} is required`]
          hasError = true
        }
      }

      if (hasError) {
        Swal.fire({
          icon: 'error',
          title: 'Validation Error',
          text: 'Please fill in all required fields',
          confirmButtonText: 'Close',
        })
        return
      }

      // Continue with the existing logic if validation passes
      Swal.fire({
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
        allowEscapeKey: false,
        allowEnterKey: false,
      })

      const valueToNumber = parseInt(this.additional.value, 10)
      const tatToNumber = parseInt(this.additional.tat, 10)

      // Extracting IDs from selected objects
      const customerId = this.additional.customer_id
        ? this.additional.customer_id.id
        : null
      const productId = this.additional.product_id
        ? this.additional.product_id.id
        : null
      const maintenanceIds = this.additional.maintenance_id
        ? this.additional.maintenance_id.map((m) => m.id)
        : []
      const acTypeId = this.additional.ac_type_id
        ? this.additional.ac_type_id.id
        : null

      this.$axios
        .post(`api/sales-create-tmb`, {
          is_rkap: 0,
          transaction_type_id: this.additional.transaction_type_id,
          customer_id: customerId,
          product_id: productId,
          maintenance_id: maintenanceIds,
          ac_type_id: acTypeId,
          ac_reg: this.additional.ac_reg,
          remarks: this.additional.remarks,
          value: valueToNumber,
          start_date: this.additional.start_date,
          tat: tatToNumber,
        })
        .then((response) => {
          Swal.close()

          Swal.fire({
            icon: 'success',
            title: 'Success!',
            text: response.data.message,
            confirmButtonText: 'Close',
            allowOutsideClick: false,
          }).then((result) => {
            if (result.isConfirmed) {
              this.closeAddSales()
            }
          })
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    showErrorResponse(error) {
      let error_icon = 'error'
      let error_title = 'Oops!'
      let error_message = "Something's wrong, failed to process the request"

      if (error.response.status == 422) {
        error_icon = 'warning'
        error_title = 'Validation Failed'
        error_message = error.response.data.errors.reason[0]
      } else if (error.response.status == 500) {
        error_title = 'Internal Server Error'
      }

      Swal.fire({
        icon: error_icon,
        title: error_title,
        text: error_message,
      })
    },

    closeModal() {
      this.clearForm()
    },
    clearForm() {
      this.area = []
      this.ams = []
      this.customer = []
      this.product = []
      this.acReg = []
      this.other = []
      this.type = []
      this.level = []
      this.progress = []
      this.status = []
    },

    closeAddSales() {
      document.getElementById('close_modal').click()
      this.clearFormAddSales()
      this.clearForm()
    },
    clearFormAddSales() {
      this.additional.customer_id = null
      this.additional.product_id = null
      this.additional.maintenance_id = null
      this.additional.ac_type_id = null
      this.additional.ac_reg = null
      this.additional.value = null
      this.additional.start_date = null
      this.additional.tat = null
      this.additional.transaction_type_id = null

      this.errors.customer_id = null
      this.errors.product_id = null
      this.errors.hangar_id = null
      this.errors.maintenance_id = null
      this.errors.ac_type_id = null
      this.errors.ac_reg = null
      this.errors.remarks = null
      this.errors.transaction_type_id = null
      this.errors.value = null
      this.errors.start_date = null
      this.errors.tat = null

      this.listTable()
    },
  },
}
</script>

<style>
.modal-filter {
  margin-top: 25%;
}

.badge-gmf {
  background-color: #003399;
}

.badge-green {
  background-color: #ade792;
}
</style>
