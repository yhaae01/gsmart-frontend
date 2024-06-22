<template>
  <div>
    <!-- Content -->
    <div class="container-fluid mt-5 mb-5">
      <!-- Breadcrumb -->
      <div class="row mb-5">
        <div class="col-lg-6">
          <Nuxt-link
            class="menu-link"
            to="/my-salesplan-table"
            active-class="active"
          >
            <i class="fa-solid fa-angle-left"></i>
            <span class="menu-title">Sales Plan Table</span>
          </Nuxt-link>
        </div>
        <div class="col-lg-6">
          <div class="position-relative">
            <div class="position-absolute top-0 end-0">
              <nav aria-label="breadcrumb">
                <ol class="breadcrumb">
                  <li class="breadcrumb-item">
                    <Nuxt-link
                      class="menu-link"
                      to="/my-salesplan-table"
                      active-class="active"
                    >
                      <span class="menu-title">Sales Plan Table</span>
                      &nbsp;
                    </Nuxt-link>
                  </li>
                  <li class="breadcrumb-item active" aria-current="page">
                    Detail
                  </li>
                </ol>
              </nav>
            </div>
          </div>
        </div>
      </div>
      <!-- End Breadcrumb -->

      <!-- Detail -->
      <div class="row mt-4">
        <div class="card">
          <div class="card-body">
            <!-- header -->
            <div class="row">
              <div class="col-lg-6">
                <h3>Detail Sales Plan</h3>
              </div>
              <div class="col-lg-6">
                <div class="text-right">
                  <div
                    class="position-absolute top-0 end-0 mx-15 mt-4"
                    v-if="sales_detail"
                  >
                    <button
                      v-if="
                        (sales_detail.status === 'Open' ||
                          sales_detail.status === 'Closed Sales') &&
                        (role == 'Administrator' || role == 'TPR')
                      "
                      class="btn btn-info btn-sm me-2"
                      data-bs-toggle="modal"
                      data-bs-target="#switchAMS"
                    >
                      <i class="fa-solid fa-repeat"></i>
                      Switch AMS
                    </button>
                    <button
                      v-if="
                        (sales_detail.type === 'TMB Project' ||
                          sales_detail.type === 'TMB Retail') &&
                        (sales_detail.status === 'Open' ||
                          sales_detail.status === 'Closed Sales') &&
                        (role === 'Administrator' ||
                          role === 'AMS' ||
                          role === 'AM')
                      "
                      class="btn btn-primary btn-sm"
                      data-bs-toggle="modal"
                      data-bs-target="#editSales"
                    >
                      <i class="fa-solid fa-pen"></i> Edit Sales Plan
                    </button>
                    <button
                      v-if="
                        (sales_detail.status === 'Open' ||
                          sales_detail.status === 'Closed Sales') &&
                        (role === 'Administrator' ||
                          role === 'AMS' ||
                          role === 'AM')
                      "
                      class="btn btn-warning btn-sm ms-2"
                      data-bs-toggle="modal"
                      data-bs-target="#revisionUpload"
                    >
                      <i class="fa-solid fa-upload"></i> Revision Upload
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <!-- End header -->

            <!-- Modal switchAMS -->
            <div
              class="modal fade"
              id="switchAMS"
              tabindex="-1"
              aria-labelledby="switchAMSLabel"
              aria-hidden="true"
              v-if="sales_detail"
            >
              <div class="modal-dialog">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title w-100" id="switchAMSLabel">
                      Switch AMS
                    </h5>
                    <button
                      type="button"
                      class="btn-close"
                      data-bs-dismiss="modal"
                      aria-label="Close"
                      @click="closeModalAMS()"
                    ></button>
                  </div>
                  <form>
                    <div class="modal-body">
                      <div class="mb-3">
                        <label for="">Current AMS</label>
                        <input
                          type="text"
                          class="form-control"
                          v-model="sales_detail.ams"
                          disabled
                        />
                      </div>
                      <form action="">
                        <div class="mb-3">
                          <label class="form-label">To</label>
                          <multiselect
                            v-model="selectedAMS"
                            :options="amsOptions"
                            :class="{ 'is-invalid': errors.ams_id }"
                            track-by="id"
                            label="displayText"
                            placeholder="Select AMS"
                          ></multiselect>
                          <span
                            v-if="errors.ams_id"
                            class="error invalid-feedback"
                            >{{ errors.ams_id[0] }}</span
                          >
                        </div>
                        <div class="mb-3">
                          <label class="form-label">Description</label> <br />
                          <div id="bodyAMS">
                            Level: <b>{{ sales_detail.level }}</b> Status:
                            <b>{{ sales_detail.status }}</b> Sales Type:
                            <b>{{ sales_detail.other }}</b> Type:
                            <b>{{ sales_detail.type }}</b> Month Sales:
                            <b>{{ sales_detail.monthSales }}</b> Year:
                            <b>{{ sales_detail.year }}</b> Start Date Project:
                            <b>{{ sales_detail.startDate }}</b> End Date
                            Project: <b>{{ sales_detail.endDate }}</b> TAT:
                            <b>{{ sales_detail.tat }} Days</b> Progress:
                            <b>{{ sales_detail.progress }}%</b> Product:
                            <b>{{ sales_detail.product.name }}</b>
                            Location:
                            <b v-if="sales_detail.location == '-' || null">-</b>
                            <b v-else>{{ sales_detail.location }}</b>
                            Maintenance:
                            <b>{{ sales_detail.maintenance }}</b>
                          </div>
                        </div>
                      </form>
                    </div>
                    <div class="modal-footer">
                      <button
                        type="button"
                        class="btn btn-secondary"
                        data-bs-dismiss="modal"
                        id="close_modal_ams"
                        @click="closeModalAMS()"
                      >
                        Close
                      </button>
                      <button
                        type="button"
                        class="btn btn-primary"
                        @click="switchAMS()"
                      >
                        Send
                      </button>
                    </div>
                  </form>
                </div>
              </div>
            </div>

            <!-- Modal edit Sales -->
            <div
              class="modal fade"
              tabindex="-1"
              id="editSales"
              data-bs-backdrop="static"
            >
              <div class="modal-dialog modal-dialog-centered modal-lg">
                <div class="modal-content">
                  <div class="modal-header">
                    <h3 class="modal-title">Edit Sales Plan</h3>

                    <!--begin::Close-->
                    <div
                      class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                      data-bs-dismiss="modal"
                      aria-label="Close"
                      @click="closeModalEditSales()"
                    >
                      <span
                        class="svg-icon svg-icon-1"
                        @click="closeModalContact()"
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
                    <form
                      @submit.prevent="salesUpdate"
                      v-if="
                        sales_detail &&
                        (sales_detail.type === 'TMB Retail' ||
                          sales_detail.type === 'TMB Project')
                      "
                    >
                      <input type="hidden" v-model="sales_detail.id" />
                      <div class="row">
                        <div class="col-lg-6">
                          <div class="form-group mb-3">
                            <label class="form-label fw-bold"
                              >Product <span class="text-danger">*</span></label
                            >
                            <multiselect
                              v-model="selectedProduct"
                              :options="productOptions"
                              :class="{ 'is-invalid': errors.product_id }"
                              track-by="id"
                              label="name"
                            ></multiselect>
                            <span
                              v-if="errors.product_id"
                              class="error invalid-feedback"
                            >
                              {{ errors.product_id[0] }}
                            </span>
                          </div>
                        </div>
                        <div class="col-lg-6">
                          <div class="form-group mb-3">
                            <label class="form-label fw-bold"
                              >Maintenance
                              <span class="text-danger">*</span></label
                            >
                            <multiselect
                              v-model="selectedMaintenance"
                              :options="maintenanceOptions"
                              :class="{ 'is-invalid': errors.maintenance_id }"
                              :searchable="true"
                              :multiple="true"
                              track-by="id"
                              label="name"
                              :disabled="selectedProduct === null"
                            ></multiselect>
                            <span
                              v-if="errors.maintenance_id"
                              class="error invalid-feedback"
                            >
                              {{ errors.maintenance_id[0] }}
                            </span>
                          </div>
                        </div>
                      </div>

                      <div class="row my-3">
                        <div class="col-lg-6">
                          <div class="form-group mb-3">
                            <label class="form-label fw-bold"
                              >Sales Plan
                              <span class="text-danger">*</span></label
                            >
                            <input
                              type="number"
                              class="form-control"
                              v-model="sales_update_value"
                              :class="{
                                'is-invalid': errors.value,
                              }"
                            />
                            <span
                              v-if="errors.value"
                              class="error invalid-feedback"
                              >{{ errors.value[0] }}</span
                            >
                          </div>
                        </div>
                        <div class="col-lg-6">
                          <div class="form-group mb-3">
                            <label class="form-label fw-bold"
                              >Registration</label
                            >
                            <input
                              type="text"
                              class="form-control"
                              v-model="sales_update_acreg"
                              :class="{
                                'is-invalid': errors.ac_reg,
                              }"
                            />
                            <span
                              v-if="errors.ac_reg"
                              class="error invalid-feedback"
                              >{{ errors.ac_reg[0] }}</span
                            >
                          </div>
                        </div>
                      </div>

                      <div class="row mt-10">
                        <div class="col d-flex justify-content-end">
                          <button
                            type="button"
                            class="btn btn-light mx-2"
                            data-bs-dismiss="modal"
                            id="close_modal_edit_sales"
                            @click="closeModalEditSales()"
                          >
                            Close
                          </button>
                          <button type="submit" class="btn btn-primary">
                            Save
                          </button>
                        </div>
                      </div>
                    </form>
                    <!-- TODO Edit PBTH ? -->
                    <form
                      class="mt-5"
                      @submit.prevent="pbthUpdate"
                      v-if="sales_detail && sales_detail.type === 'PBTH'"
                    >
                      <h3 class="fs-2 fw-fold">Edit Rate & Flight Hour</h3>
                      <div class="row">
                        <div class="col-lg-6">
                          <div class="form-group mb-3">
                            <label class="form-label fw-bold"> Rate </label>
                            <input
                              type="number"
                              class="form-control"
                              v-model="data_pbth_rate"
                            />
                            <span
                              v-if="errors.maintenance_id"
                              class="error invalid-feedback"
                            >
                              {{ errors.maintenance_id[0] }}
                            </span>
                          </div>
                        </div>
                        <div class="col-lg-6">
                          <div class="form-group mb-3">
                            <label class="form-label fw-bold">
                              Flight Hour
                            </label>
                            <input
                              type="number"
                              class="form-control"
                              v-model="data_pbth_flighthour"
                            />
                            <span
                              v-if="errors.hangar_id"
                              class="error invalid-feedback"
                            >
                              {{ errors.hangar_id[0] }}
                            </span>
                          </div>
                        </div>
                      </div>
                      <div class="row mt-10">
                        <div class="col d-flex justify-content-end">
                          <button
                            type="button"
                            class="btn btn-light mx-2"
                            data-bs-dismiss="modal"
                            id="close_modal_edit_sales"
                          >
                            Close
                          </button>
                          <button type="submit" class="btn btn-primary">
                            Save
                          </button>
                        </div>
                      </div>
                    </form>
                  </div>
                </div>
              </div>
            </div>

            <!-- Modal Revision Upload -->
            <div
              class="modal fade"
              id="revisionUpload"
              tabindex="-1"
              data-bs-backdrop="static"
            >
              <div class="modal-dialog modal-dialog-centered modal-lg">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title">Upload Revision File</h5>
                    <!--begin::Close-->
                    <div
                      class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                      data-bs-dismiss="modal"
                      aria-label="Close"
                      id="close_modal_file_revision"
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
                    <div class="mb-3">
                      <label for="selectLevel" class="form-label"
                        >Select Level:</label
                      >
                      <select
                        id="selectLevel"
                        v-model="selectedLevel"
                        @change="handleLevelChange"
                        class="form-select"
                        v-if="sales_detail"
                      >
                        <option v-if="!selectedLevel" value="">
                          Open this select menu
                        </option>
                        <option v-if="sales_detail.level <= 1" :value="1">
                          Level 1
                        </option>
                        <option v-if="sales_detail.level <= 2" :value="2">
                          Level 2
                        </option>
                        <option v-if="sales_detail.level <= 3" :value="3">
                          Level 3
                        </option>
                        <option v-if="sales_detail.level <= 4" :value="4">
                          Level 4
                        </option>
                      </select>
                    </div>
                    <div class="mb-3">
                      <label for="selectOption" class="form-label"
                        >Select Option:</label
                      >
                      <select
                        id="selectOption"
                        v-model="sequence"
                        class="form-select"
                        :disabled="!selectedLevel"
                      >
                        <option value="" v-if="!selectedLevel">
                          Select sales level first
                        </option>
                        <option value="" v-else>Open this select menu</option>
                        <option
                          v-for="option in secondOptions"
                          :value="option.value"
                          :key="option.value"
                        >
                          {{ option.label }}
                        </option>
                      </select>
                    </div>
                    <div class="mb-3">
                      <label for="selectOption" class="form-label"
                        >Upload File:</label
                      >
                      <input
                        type="file"
                        @change="attachFileRevision"
                        id="filesRevision"
                        ref="filesRevision"
                        class="form-control"
                        multiple
                        :class="{
                          'is-invalid': errors.filesRevision,
                        }"
                      />
                      <div v-for="(file_error, key) in file_errors" :key="key">
                        <ul class="list-group mt-3">
                          <li
                            class="list-group-item text-danger border-0"
                            v-for="(errorItem, innerKey) in file_error"
                            :key="innerKey"
                          >
                            {{ errorItem }}
                          </li>
                        </ul>
                      </div>
                    </div>
                  </div>
                  <div class="modal-footer">
                    <button
                      class="btn btn-secondary"
                      @click="closeModalFileRevision()"
                    >
                      Close
                    </button>
                    <button
                      type="button"
                      class="btn btn-primary"
                      @click="submitFileRevision()"
                    >
                      Submit
                    </button>
                  </div>
                </div>
              </div>
            </div>

            <!-- Card -->
            <div class="row mt-8">
              <div class="col-lg-4 col-md-12 col-sm-12">
                <div class="card mb-3" style="max-width: 540px">
                  <div class="row no-gutters">
                    <div
                      class="col-md"
                      style="margin-top: -20px; margin-left: -30px"
                    >
                      <div class="card-body">
                        <h5 class="card-title" v-if="sales_detail">
                          {{ sales_detail.registration }}
                        </h5>
                        <p class="card-text" v-if="sales_detail">
                          <small class="text-muted"
                            ><i class="fa-solid fa-plane-up"></i>
                            {{ sales_detail.acReg }}</small
                          >
                        </p>
                        <p class="card-text" v-if="sales_detail">
                          <small class="text-muted"
                            ><i class="fa-solid fa-spa"></i>
                            {{ sales_detail.customer.name }}</small
                          >
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="col-lg">
                <div class="text-center d-grid gap-2">
                  <span class="me-2 mb-2" id="cardSalesPlan">
                    <h3 class="mt-2" id="textSalesPlan" v-if="sales_detail">
                      USD {{ formatPrice(sales_detail.totalSales) }}
                    </h3>
                    <p><small class="text-muted">Sales Plan</small></p>
                  </span>
                </div>
              </div>
              <div class="col-lg">
                <div class="text-center d-grid gap-2">
                  <span class="me-2 mb-2" id="cardMarketShare">
                    <h3 class="mt-2" id="textMarketShare" v-if="sales_detail">
                      USD {{ formatPrice(sales_detail.salesRkap) }}
                    </h3>
                    <p><small class="text-muted">Sales RKAP</small></p>
                  </span>
                </div>
              </div>
              <div class="col-lg">
                <div class="text-center d-grid gap-2">
                  <span class="me-2 mb-2" id="cardDevisiasi">
                    <h3 class="mt-2" id="textDevisiasi" v-if="sales_detail">
                      USD {{ formatPrice(sales_detail.deviasi) }}
                    </h3>
                    <p><small class="text-muted">Deviation</small></p>
                  </span>
                </div>
              </div>
            </div>
            <!-- End Card -->

            <!-- Detail -->
            <div class="row" v-if="sales_detail">
              <div class="col-lg-1 col-s">
                <p class="text-muted mt-5">Level</p>
                <div>
                  <span
                    :class="{
                      'badge badge-success': sales_detail.level === 1,
                      'badge badge-warning': sales_detail.level === 2,
                      'badge badge-primary': sales_detail.level === 3,
                      'badge badge-danger': sales_detail.level === 4,
                    }"
                  >
                    <b>Level {{ sales_detail.level }}</b>
                  </span>
                </div>
                <p class="text-muted mt-5">Status</p>
                <div>
                  <span
                    v-if="sales_detail.status === 'Cancel'"
                    class="badge badge-danger"
                    ><b>{{ sales_detail.status }}</b></span
                  >
                  <span
                    v-else-if="sales_detail.status === 'Open'"
                    class="badge badge-gmf"
                    ><b>{{ sales_detail.status }}</b></span
                  >
                  <span
                    v-else-if="sales_detail.status === 'Closed Sales'"
                    class="badge badge-green"
                    ><b>{{ sales_detail.status }}</b></span
                  >
                  <span
                    v-else-if="sales_detail.status === 'Closed In'"
                    class="badge badge-success"
                    ><b>{{ sales_detail.status }}</b></span
                  >
                </div>
              </div>
              <div class="col-lg-1">
                <p class="text-muted mt-5">Sales Type</p>
                <div>
                  <b>{{ sales_detail.other }}</b>
                </div>
                <p class="text-muted mt-5">Type</p>
                <div>
                  <b>{{ sales_detail.type }}</b>
                </div>
              </div>
              <div class="col-lg-2">
                <p class="text-muted mt-5">Progress</p>
                <div
                  v-if="
                    sales_detail.progress >= 1 && sales_detail.progress <= 30
                  "
                >
                  <span class="badge badge-danger"
                    >{{ sales_detail.progress }} %</span
                  >
                </div>
                <div
                  v-if="
                    sales_detail.progress >= 31 && sales_detail.progress <= 60
                  "
                >
                  <span class="badge badge-warning"
                    >{{ sales_detail.progress }} %</span
                  >
                </div>
                <div
                  v-if="
                    sales_detail.progress >= 61 && sales_detail.progress <= 80
                  "
                >
                  <span class="badge badge-primary"
                    >{{ sales_detail.progress }} %</span
                  >
                </div>
                <div
                  v-if="
                    sales_detail.progress >= 81 && sales_detail.progress <= 100
                  "
                >
                  <span class="badge badge-success"
                    >{{ sales_detail.progress }} %</span
                  >
                </div>
                <p class="text-muted mt-5">Month Sales</p>
                <p>
                  <b>{{ sales_detail.monthSales }}</b>
                </p>
              </div>

              <div class="col-lg-1">
                <p class="text-muted mt-5">TAT</p>
                <p>
                  <b>{{ sales_detail.tat }} Days</b>
                </p>
                <p class="text-muted mt-5">Year</p>
                <p>
                  <b>{{ sales_detail.year }}</b>
                </p>
              </div>
              <div class="col-lg-2">
                <p class="text-muted mt-5">Start Date Project</p>
                <p>
                  <b>{{
                    moment(sales_detail.startDate).format('DD-MM-YYYY')
                  }}</b>
                </p>
                <p class="text-muted mt-5">End Date Project</p>
                <p>
                  <b>{{ moment(sales_detail.endDate).format('DD-MM-YYYY') }}</b>
                </p>
              </div>
              <div class="col-lg-2">
                <p class="text-muted mt-5">Location</p>
                <p>
                  <b>{{ sales_detail?.location ?? '-' }}</b>
                </p>
                <p class="text-muted mt-5">Product</p>
                <p>
                  <b>{{ sales_detail.product.name }}</b>
                </p>
              </div>
              <div class="col-lg-3">
                <p class="text-muted mt-5">Maintenance</p>
                <p>
                  <b>{{ sales_detail?.maintenance ?? '-' }}</b>
                </p>
                <p class="text-muted mt-5">SO Number</p>
                <p>
                  <b>{{ sales_detail?.so_number ?? '-' }}</b>
                </p>
              </div>
            </div>
            <!-- End Detail -->
            <hr />

            <!-- Tab -->
            <div class="row mt-5">
              <ul class="nav nav-pills" id="pills-tab" role="tablist">
                <li class="nav-item" role="presentation">
                  <button
                    class="nav-link active"
                    id="upgrade-level-tab"
                    data-bs-toggle="tab"
                    data-bs-target="#upgrade-level-tab-pane"
                    type="button"
                    role="tab"
                    aria-controls="upgrade-level-tab-pane"
                    aria-selected="true"
                  >
                    Upgrade Level
                    <span
                      v-if="
                        (request_upgrade?.status == 2 &&
                          (request_upgrade?.reviewer_id == uid ||
                            role == 'Administrator' ||
                            role == 'TPR')) ||
                        (request_upgrade?.status == 4 &&
                          (role == 'AMS' || role == 'AM')) ||
                        (request_hangar?.status == 2 &&
                          (request_hangar?.reviewer_id == uid ||
                            role == 'Administrator' ||
                            role == 'TPR')) ||
                        (request_hangar?.status == 4 &&
                          (role == 'AMS' || role == 'AM')) ||
                        (request_closed?.status == 2 &&
                          (request_closed?.reviewer_id == uid ||
                            role == 'Administrator' ||
                            role == 'TPR')) ||
                        (request_closed?.status == 4 &&
                          (role == 'AMS' || role == 'AM'))
                      "
                      class="spinner-grow spinner-grow-sm ms-1"
                      role="status"
                      aria-hidden="true"
                    >
                    </span>
                  </button>
                </li>
                <li class="nav-item" role="presentation">
                  <button
                    class="nav-link"
                    id="history-tab"
                    data-bs-toggle="tab"
                    data-bs-target="#history-tab-pane"
                    type="button"
                    role="tab"
                    aria-controls="history-tab-pane"
                    aria-selected="false"
                  >
                    History
                  </button>
                </li>
                <li class="nav-item" role="presentation">
                  <button
                    class="nav-link"
                    id="contact-tab"
                    data-bs-toggle="tab"
                    data-bs-target="#contact-tab-pane"
                    type="button"
                    role="tab"
                    aria-controls="contact-tab-pane"
                    aria-selected="false"
                  >
                    Contact Person
                  </button>
                </li>
                <li
                  class="nav-item"
                  role="presentation"
                  v-if="
                    (role == 'AMS' ||
                      role == 'AM' ||
                      role == 'Administrator' ||
                      role == 'TPR' ||
                      role == 'CBO') &&
                    (sales_detail_status == 'Open' ||
                      sales_detail_status == 'Closed Sales')
                  "
                >
                  <button
                    class="nav-link"
                    id="reschedule-tab"
                    data-bs-toggle="tab"
                    data-bs-target="#reschedule-tab-pane"
                    type="button"
                    role="tab"
                    aria-controls="reschedule-tab-pane"
                    aria-selected="false"
                  >
                    Reschedule
                    <span
                      v-if="
                        (sales_detail.type == 'TMB Project' ||
                          sales_detail.type == 'TMB Retail') &&
                        sales_reschedule &&
                        request_reschedule &&
                        ((request_reschedule?.status == 2 &&
                          (request_reschedule?.reviewer_id == uid ||
                            role == 'Administrator' ||
                            role == 'TPR')) ||
                          (request_reschedule?.status == 4 &&
                            (role == 'AMS' || role == 'AM')))
                      "
                      class="spinner-grow spinner-grow-sm ms-1"
                      role="status"
                      aria-hidden="true"
                    >
                    </span>
                  </button>
                </li>
                <li
                  class="nav-item"
                  role="presentation"
                  v-if="
                    (role == 'AMS' ||
                      role == 'AM' ||
                      role == 'Administrator' ||
                      role == 'TPR') &&
                    (sales_detail_status == 'Open' ||
                      sales_detail_status == 'Closed Sales')
                  "
                >
                  <button
                    class="nav-link"
                    id="cancel-tab"
                    data-bs-toggle="tab"
                    data-bs-target="#cancel-tab-pane"
                    type="button"
                    role="tab"
                    aria-controls="cancel-tab-pane"
                    aria-selected="false"
                  >
                    Cancel
                    <span
                      v-if="
                        request_cancel &&
                        ((request_cancel?.status == 2 &&
                          (request_reschedule?.reviewer_id == uid ||
                            role == 'Administrator' ||
                            role == 'TPR')) ||
                          (request_cancel?.status == 4 &&
                            (role == 'AMS' || role == 'AM')))
                      "
                      class="spinner-grow spinner-grow-sm ms-1"
                      role="status"
                      aria-hidden="true"
                    >
                    </span>
                  </button>
                </li>
              </ul>

              <div class="tab-content" id="pills-tabContent">
                <!-- Tab Upgrade Level -->
                <div
                  class="tab-pane fade show active"
                  id="upgrade-level-tab-pane"
                  role="tabpanel"
                  aria-labelledby="upgrade-level-tab"
                  tabindex="0"
                >
                  <!--begin::Stepper-->
                  <div
                    class="stepper stepper-pills mt-5"
                    id="kt_stepper_example_basic"
                  >
                    <div v-if="sales_detail">
                      <!--begin::Nav-->
                      <div v-if="sales_detail.level == 4">
                        <div class="stepper-nav flex-center flex-wrap mb-10">
                          <!--begin::Step 1-->
                          <div
                            class="stepper-item mx-8 my-4 current"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">4</span>
                              </div>
                              <!--end::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Awareness</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 1-->
                          <!--begin::Step 2-->
                          <div
                            class="stepper-item mx-8 my-4"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">3</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Opportunity</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 2-->

                          <!--begin::Step 3-->
                          <div
                            class="stepper-item mx-8 my-4"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">2</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">
                                  Attractive Proposal
                                </div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 3-->

                          <!--begin::Step 4-->
                          <div
                            class="stepper-item mx-8 my-4"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">1</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Contract Signing</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 4-->
                        </div>
                      </div>
                      <div v-else-if="sales_detail.level == 3">
                        <div class="stepper-nav flex-center flex-wrap mb-10">
                          <!--begin::Step 1-->
                          <div
                            class="stepper-item mx-8 my-4 completed"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">4</span>
                              </div>
                              <!--end::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Awareness</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 1-->
                          <!--begin::Step 2-->
                          <div
                            class="stepper-item mx-8 my-4 current"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">3</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Opportunity</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 2-->

                          <!--begin::Step 3-->
                          <div
                            class="stepper-item mx-8 my-4"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">2</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">
                                  Attractive Proposal
                                </div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 3-->

                          <!--begin::Step 4-->
                          <div
                            class="stepper-item mx-8 my-4"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">1</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Contract Signing</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 4-->
                        </div>
                      </div>
                      <div v-else-if="sales_detail.level == 2">
                        <div class="stepper-nav flex-center flex-wrap mb-10">
                          <!--begin::Step 1-->
                          <div
                            class="stepper-item mx-8 my-4 completed"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">4</span>
                              </div>
                              <!--end::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Awareness</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 1-->
                          <!--begin::Step 2-->
                          <div
                            class="stepper-item mx-8 my-4 completed"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">3</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Opportunity</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 2-->

                          <!--begin::Step 3-->
                          <div
                            class="stepper-item mx-8 my-4 current"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">2</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">
                                  Attractive Proposal
                                </div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 3-->

                          <!--begin::Step 4-->
                          <div
                            class="stepper-item mx-8 my-4"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">1</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Contract Signing</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 4-->
                        </div>
                      </div>
                      <div v-else-if="sales_detail.level == 1">
                        <div class="stepper-nav flex-center flex-wrap mb-10">
                          <!--begin::Step 1-->
                          <div
                            class="stepper-item mx-8 my-4 completed"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">4</span>
                              </div>
                              <!--end::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Awareness</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 1-->
                          <!--begin::Step 2-->
                          <div
                            class="stepper-item mx-8 my-4 completed"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">3</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Opportunity</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 2-->

                          <!--begin::Step 3-->
                          <div
                            class="stepper-item mx-8 my-4 completed"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">2</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">
                                  Attractive Proposal
                                </div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 3-->

                          <!--begin::Step 4-->
                          <div
                            class="stepper-item mx-8 my-4 current"
                            data-kt-stepper-element="nav"
                          >
                            <!--begin::Wrapper-->
                            <div
                              class="stepper-wrapper d-flex align-items-center"
                            >
                              <!--begin::Icon-->
                              <div class="stepper-icon w-40px h-40px">
                                <i class="stepper-check fas fa-check"></i>
                                <span class="stepper-number">1</span>
                              </div>
                              <!--begin::Icon-->

                              <!--begin::Label-->
                              <div class="stepper-label">
                                <div class="stepper-desc">Contract Signing</div>
                              </div>
                              <!--end::Label-->
                            </div>
                            <!--end::Wrapper-->

                            <!--begin::Line-->
                            <div class="stepper-line h-40px"></div>
                            <!--end::Line-->
                          </div>
                          <!--end::Step 4-->
                        </div>
                      </div>
                      <!--end::Nav-->
                    </div>

                    <!--begin::Form-->
                    <div
                      class="form mx-auto"
                      novalidate="novalidate"
                      id="kt_stepper_example_basic_div"
                    >
                      <!--begin::Group-->
                      <div class="mb-5" v-if="sales_detail">
                        <!-- NOTE: in case Sales Plan status is in progress -->
                        <div
                          v-if="
                            sales_detail_status == 'Open' ||
                            sales_detail_status == 'Closed Sales'
                          "
                        >
                          <!--begin::Step 4-->
                          <div v-if="sales_detail.level == 4">
                            <div
                              class="flex-column current"
                              data-kt-stepper-element="content"
                            >
                              <form>
                                <div class="row">
                                  <!-- Fill in Contact Person of Customer -->
                                  <div class="col-lg-6">
                                    <h3>Fill in Contact Person of Customer</h3>
                                  </div>
                                  <div class="col-lg-6 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 4 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addContact"
                                          @click="addContact()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Add Contact Person
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3 table-responsive">
                                      <table class="table" v-if="contact">
                                        <tr
                                          v-for="contact in level4[0].data"
                                          :key="contact.id"
                                          id="heightContact"
                                        >
                                          <td>
                                            <strong>{{ contact.name }}</strong>
                                          </td>
                                          <td>
                                            <strong>{{ contact.phone }}</strong>
                                          </td>
                                          <td>
                                            <strong>{{ contact.email }}</strong>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <!-- Upload Attachment RFQ or Email Request -->
                                  <div class="col-lg-6 mt-3">
                                    <h3>
                                      Upload Attachment RFQ or Email Request
                                    </h3>
                                    <p class="text-danger">
                                      <small>* File size max 5MB</small>
                                    </p>
                                  </div>
                                  <div class="col-lg-6 mt-3 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 4 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addFile"
                                          @click="addFile1()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Upload Document
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3 table-responsive">
                                      <table class="table" v-if="files">
                                        <tr
                                          v-for="files in level4[1].data"
                                          :key="files.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="files.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  files.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                uploaded by
                                                <a href="#">{{
                                                  files.uploader_name
                                                }}</a>
                                              </p>
                                            </div>
                                          </td>
                                          <td
                                            class="d-flex justify-content-end"
                                            v-if="
                                              sales_detail.level == 4 &&
                                              sales_detail.status === 'Open'
                                            "
                                          >
                                            <button
                                              type="button"
                                              class="btn btn-danger btn-sm"
                                              @click="removeFile(files.id)"
                                              v-if="
                                                (role == 'AMS' ||
                                                  role == 'AM' ||
                                                  role == 'Administrator') &&
                                                (request_upgrade == null ||
                                                  request_upgrade?.status == 1)
                                              "
                                            >
                                              <span class="fas fa-trash"></span>
                                            </button>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <!-- Upload Attachment Workscope -->
                                  <div class="col-lg-6 mt-3">
                                    <h3>Upload Attachment Workscope</h3>
                                    <p class="text-danger">
                                      <small>* File size max 5MB</small>
                                    </p>
                                  </div>
                                  <div class="col-lg-6 mt-3 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 4 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addFile"
                                          @click="addFile2()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Upload Document
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3">
                                      <table class="table" v-if="files">
                                        <tr
                                          v-for="files in level4[2].data"
                                          :key="files.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="files.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  files.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                uploaded by
                                                <a href="#">{{
                                                  files.uploader_name
                                                }}</a>
                                              </p>
                                            </div>
                                          </td>
                                          <td
                                            class="d-flex justify-content-end"
                                            v-if="
                                              sales_detail.level == 4 &&
                                              sales_detail.status === 'Open'
                                            "
                                          >
                                            <button
                                              type="button"
                                              class="btn btn-danger btn-sm"
                                              @click="removeFile(files.id)"
                                              v-if="
                                                (role == 'AMS' ||
                                                  role == 'AM' ||
                                                  role == 'Administrator') &&
                                                (request_upgrade == null ||
                                                  request_upgrade?.status == 1)
                                              "
                                            >
                                              <span class="fas fa-trash"></span>
                                            </button>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <!-- Slot Request -->
                                  <div class="col-lg-6 mt-5">
                                    <h3>Slot Request</h3>
                                  </div>
                                  <div class="col-lg-6 mt-5 mb-20">
                                    <div class="position-relative">
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="sales_detail.status === 'Open'"
                                      >
                                        <!-- Start of Hangar Slot Status -->
                                        <!-- NOTE: Info for users other than requester & reviewer -->
                                        <div
                                          v-if="
                                            level4[3].data != null &&
                                            (request_hangar?.status == 2 ||
                                              request_hangar?.status == 4) &&
                                            request_hangar?.reviewer_id !=
                                              uid &&
                                            (role == 'TP' || role == 'TPC')
                                          "
                                          class="alert alert-warning fw-bold"
                                          role="alert"
                                          style="color: #e6b005"
                                        >
                                          This Hangar Slot is under review by
                                          {{ request_hangar?.reviewer_name }}
                                        </div>

                                        <!-- NOTE: Info for requester (Waiting) -->
                                        <div
                                          v-if="
                                            level4[3].data != null &&
                                            request_hangar?.status == 2 &&
                                            (role == 'AMS' || role == 'AM')
                                          "
                                          class="alert alert-info"
                                          role="alert"
                                        >
                                          Hangar Slot requested, waiting for
                                          approval
                                        </div>

                                        <!-- NOTE: Info for requester (Approved) -->
                                        <div
                                          v-if="
                                            level4[3].data != null &&
                                            request_hangar?.status == 3 &&
                                            request_hangar?.commit == false &&
                                            (role == 'AMS' || role == 'AM')
                                          "
                                          class="alert alert-success alert-dismissible d-flex align-items-center"
                                          role="alert"
                                        >
                                          <span>
                                            Your Hangar Slot request has been
                                            <strong>approved</strong>
                                          </span>
                                          <button
                                            type="button"
                                            class="btn-close"
                                            @click="responseHangarRequest()"
                                          ></button>
                                        </div>

                                        <!-- NOTE: Info for requester (Rejected) -->
                                        <div
                                          v-if="
                                            level4[3].data == null &&
                                            request_hangar?.status == 4 &&
                                            (role == 'AMS' || role == 'AM')
                                          "
                                          class="alert alert-danger alert-dismissible d-flex align-items-center"
                                          role="alert"
                                        >
                                          <span>
                                            Your Hangar Slot request has been
                                            <strong>rejected</strong> with
                                            reason
                                            <strong
                                              >"{{
                                                request_hangar?.reject_reason ??
                                                '-'
                                              }}"</strong
                                            >
                                          </span>
                                          <button
                                            type="button"
                                            class="btn-close"
                                            @click="responseHangarRequest()"
                                          ></button>
                                        </div>
                                        <!-- End of Hangar Slot Status -->

                                        <!-- Start of Hangar Slot Button -->
                                        <!-- NOTE: CTA for reviewer -->
                                        <div
                                          v-if="
                                            level4[3].data != null &&
                                            (request_hangar?.reviewer_id ==
                                              uid ||
                                              role == 'Administrator' ||
                                              role == 'TPR') &&
                                            request_hangar?.status == 2
                                          "
                                        >
                                          <button
                                            type="button"
                                            @click="slotReject()"
                                            class="btn btn-danger btn-sm me-3"
                                          >
                                            Reject
                                          </button>
                                          <button
                                            type="button"
                                            class="btn btn-success btn-sm"
                                            @click="slotApprove()"
                                          >
                                            Approve
                                          </button>
                                        </div>

                                        <!-- NOTE: CTA for requester -->
                                        <button
                                          v-if="
                                            level4[3].data == null &&
                                            (role == 'AMS' || role == 'AM') &&
                                            (request_hangar?.status == 1 ||
                                              request_hangar == null) &&
                                            (request_upgrade == null ||
                                              request_upgrade?.status == 1)
                                          "
                                          type="button"
                                          class="btn btn-info btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#slotRequest"
                                        >
                                          Request Hangar Slot
                                        </button>
                                        <!-- End of Hangar Slot Button -->
                                      </div>
                                    </div>
                                  </div>
                                  <div class="row">
                                    <div class="col-lg-12">
                                      <div class="mb-3">
                                        <label>Hangar</label>
                                        <div v-if="level4[3].data != null">
                                          <input
                                            type="text"
                                            class="form-control form-control-solid"
                                            v-model="level4[3].data.hangar"
                                            readonly
                                          />
                                        </div>
                                        <div v-else>
                                          <input
                                            type="text"
                                            class="form-control form-control-solid"
                                            value="-"
                                            readonly
                                          />
                                        </div>
                                      </div>
                                    </div>
                                  </div>

                                  <div
                                    v-if="
                                      sales_detail.status === 'Open' &&
                                      sales_detail.level == 4 &&
                                      sales_detail.upgrade == true
                                    "
                                    class="text-center mt-10"
                                  >
                                    <!-- Start of Upgrade Level Status -->
                                    <!-- NOTE: Info for users other than requester & reviewer -->
                                    <div
                                      v-if="
                                        (role == 'TP' ||
                                          role == 'TPC' ||
                                          role == 'CBO') &&
                                        request_upgrade?.reviewer_id != uid &&
                                        request_upgrade?.status == 2
                                      "
                                      class="alert alert-warning fw-bold"
                                      role="alert"
                                      style="color: #e6b005"
                                    >
                                      This Sales Plan is under review by
                                      {{ request_upgrade?.reviewer_name }} to
                                      upgrade its level
                                    </div>

                                    <!-- NOTE: Info for other users than requester (AMS) -->
                                    <div
                                      v-if="
                                        (role == 'TP' ||
                                          role == 'TPC' ||
                                          role == 'CBO' ||
                                          role == 'TPR' ||
                                          role == 'Administrator') &&
                                        (request_upgrade?.status == 1 ||
                                          request_upgrade?.status == 4 ||
                                          request_upgrade == null) &&
                                        request_hangar?.status != 2
                                      "
                                      class="alert alert-info fw-bold"
                                      role="alert"
                                    >
                                      No request created yet by AMS to upgrade
                                      the Sales Plan level
                                    </div>

                                    <!-- NOTE: Info for requester (Waiting) -->
                                    <div
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_upgrade?.status == 2
                                      "
                                      class="alert alert-info fw-bold"
                                      role="alert"
                                    >
                                      Sales Plan upgrade level requested,
                                      waiting for approval
                                    </div>

                                    <!-- NOTE: Info for requester (Rejected) -->
                                    <div
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_upgrade?.status == 4
                                      "
                                      class="alert alert-danger d-flex align-items-center justify-content-between"
                                      role="alert"
                                    >
                                      <span
                                        class="fas fa-triangle-exclamation text-danger display-6"
                                      ></span>
                                      <span>
                                        Your upgrade level request has been
                                        <strong>rejected</strong> with reason
                                        <strong
                                          >"{{
                                            request_upgrade?.reject_reason ??
                                            '-'
                                          }}"</strong
                                        >
                                      </span>
                                      <button
                                        type="button"
                                        class="btn btn-danger btn-sm"
                                        @click="responseUpgradeRequest()"
                                      >
                                        Confirm
                                      </button>
                                    </div>
                                    <!-- End of Upgrade Level Status -->

                                    <!-- Start of Upgrade Level Button -->
                                    <!-- NOTE: CTA for reviewer -->
                                    <div
                                      v-if="
                                        (request_upgrade?.reviewer_id == uid ||
                                          role == 'Administrator' ||
                                          role == 'TPR') &&
                                        request_upgrade?.status == 2
                                      "
                                    >
                                      <button
                                        type="button"
                                        class="btn btn-danger btn-sm me-3"
                                        @click="upgradeReject()"
                                      >
                                        Reject Upgrade
                                      </button>
                                      <button
                                        type="button"
                                        class="btn btn-success btn-sm"
                                        @click="upgradeApprove()"
                                      >
                                        Approve Upgrade
                                      </button>
                                    </div>

                                    <!-- NOTE: CTA for requester -->
                                    <button
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_hangar?.status != 2 &&
                                        (request_upgrade?.status == 1 ||
                                          request_upgrade == null)
                                      "
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      data-bs-toggle="modal"
                                      data-bs-target="#notifyUpgrade"
                                    >
                                      Request to Upgrade Level
                                    </button>
                                    <!-- End of Upgrade Level Button -->
                                  </div>
                                </div>
                              </form>
                            </div>
                          </div>
                          <!--begin::Step 4-->

                          <!--begin::Step 3-->
                          <div v-else-if="sales_detail.level == 3">
                            <div
                              class="flex-column current"
                              data-kt-stepper-element="content"
                            >
                              <form>
                                <div class="row">
                                  <!-- Attachment of Maintenance Proposal for Customer -->
                                  <div class="col-lg-6 mt-3">
                                    <h3>
                                      Attachment of Maintenance Proposal for
                                      Customer
                                    </h3>
                                    <p class="text-danger">
                                      <small>* File size max 5MB</small>
                                    </p>
                                  </div>
                                  <div class="col-lg-6 mt-3 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 3 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addFile"
                                          @click="addFile4()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Upload Proposal
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3">
                                      <table class="table" v-if="files">
                                        <tr
                                          v-for="files in level3[1].data"
                                          :key="files.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="files.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  files.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                uploaded by
                                                <a href="#">{{
                                                  files.uploader_name
                                                }}</a>
                                              </p>
                                            </div>
                                          </td>
                                          <td
                                            class="d-flex justify-content-end"
                                            v-if="
                                              sales_detail.level == 3 &&
                                              sales_detail.status === 'Open'
                                            "
                                          >
                                            <button
                                              type="button"
                                              class="btn btn-danger btn-sm"
                                              @click="removeFile(files.id)"
                                              v-if="
                                                (role == 'AMS' ||
                                                  role == 'AM' ||
                                                  role == 'Administrator') &&
                                                (request_upgrade == null ||
                                                  request_upgrade?.status == 1)
                                              "
                                            >
                                              <span class="fas fa-trash"></span>
                                            </button>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <!-- Attachment of Profitability Analysis Form Signed -->
                                  <div class="col-lg-6 mt-3">
                                    <h3>
                                      Attachment of Profitability Analysis Form
                                      Signed
                                    </h3>
                                    <p class="text-danger">
                                      <small>* File size max 5MB</small>
                                    </p>
                                  </div>
                                  <div class="col-lg-6 mt-3 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 3 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addFile"
                                          @click="addFile5()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Upload Document
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3">
                                      <table class="table" v-if="files">
                                        <tr
                                          v-for="files in level3[2].data"
                                          :key="files.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="files.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  files.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                uploaded by
                                                <a href="#">{{
                                                  files.uploader_name
                                                }}</a>
                                              </p>
                                            </div>
                                          </td>
                                          <td
                                            class="d-flex justify-content-end"
                                            v-if="
                                              sales_detail.level == 3 &&
                                              sales_detail.status === 'Open'
                                            "
                                          >
                                            <button
                                              type="button"
                                              class="btn btn-danger btn-sm"
                                              @click="removeFile(files.id)"
                                              v-if="
                                                (role == 'AMS' ||
                                                  role == 'AM' ||
                                                  role == 'Administrator') &&
                                                (request_upgrade == null ||
                                                  request_upgrade?.status == 1)
                                              "
                                            >
                                              <span class="fas fa-trash"></span>
                                            </button>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <!-- Attachment of Financial Assesment Form (optional) -->
                                  <div class="col-lg-6 mt-3">
                                    <h3>
                                      Attachment of Financial Assesment Form
                                      (optional)
                                    </h3>
                                    <p class="text-danger">
                                      <small>* File size max 5MB</small>
                                    </p>
                                  </div>
                                  <div class="col-lg-6 mt-3 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 3 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addFile"
                                          @click="addFile3()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Upload Document
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3">
                                      <table class="table" v-if="files">
                                        <tr
                                          v-for="files in level3[0].data"
                                          :key="files.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="files.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  files.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                uploaded by
                                                <a href="#">{{
                                                  files.uploader_name
                                                }}</a>
                                              </p>
                                            </div>
                                          </td>
                                          <td
                                            class="d-flex justify-content-end"
                                            v-if="
                                              sales_detail.level == 3 &&
                                              sales_detail.status === 'Open'
                                            "
                                          >
                                            <button
                                              type="button"
                                              class="btn btn-danger btn-sm"
                                              @click="removeFile(files.id)"
                                              v-if="
                                                (role == 'AMS' ||
                                                  role == 'AM' ||
                                                  role == 'Administrator') &&
                                                (request_upgrade == null ||
                                                  request_upgrade?.status == 1)
                                              "
                                            >
                                              <span class="fas fa-trash"></span>
                                            </button>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <div
                                    v-if="
                                      sales_detail.status === 'Open' &&
                                      sales_detail.level == 3 &&
                                      sales_detail.upgrade == true
                                    "
                                    class="text-center mt-10"
                                  >
                                    <!-- Start of Upgrade Level Status -->
                                    <!-- NOTE: Info for users other than requester & reviewer -->
                                    <div
                                      v-if="
                                        (role == 'TP' ||
                                          role == 'TPC' ||
                                          role == 'CBO') &&
                                        request_upgrade?.reviewer_id != uid &&
                                        request_upgrade?.status == 2
                                      "
                                      class="alert alert-warning fw-bold"
                                      role="alert"
                                      style="color: #e6b005"
                                    >
                                      This Sales Plan is under review by
                                      {{ request_upgrade?.reviewer_name }} to
                                      upgrade its level
                                    </div>

                                    <!-- NOTE: Info for other users than requester (AMS) -->
                                    <div
                                      v-if="
                                        (role == 'TP' ||
                                          role == 'TPC' ||
                                          role == 'CBO' ||
                                          role == 'TPR' ||
                                          role == 'Administrator') &&
                                        (request_upgrade?.status == 1 ||
                                          request_upgrade?.status == 4 ||
                                          request_upgrade == null)
                                      "
                                      class="alert alert-info fw-bold"
                                      role="alert"
                                    >
                                      No request created yet by AMS to upgrade
                                      the Sales Plan level
                                    </div>

                                    <!-- NOTE: Info for requester (Waiting) -->
                                    <div
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_upgrade?.status == 2
                                      "
                                      class="alert alert-info fw-bold"
                                      role="alert"
                                    >
                                      Sales Plan upgrade level requested,
                                      waiting for approval
                                    </div>

                                    <!-- NOTE: Info for requester (Rejected) -->
                                    <div
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_upgrade?.status == 4
                                      "
                                      class="alert alert-danger d-flex align-items-center justify-content-between"
                                      role="alert"
                                    >
                                      <span
                                        class="fas fa-triangle-exclamation text-danger display-6"
                                      ></span>
                                      <span>
                                        Your upgrade level request has been
                                        <strong>rejected</strong> with reason
                                        <strong
                                          >"{{
                                            request_upgrade?.reject_reason ??
                                            '-'
                                          }}"</strong
                                        >
                                      </span>
                                      <button
                                        type="button"
                                        class="btn btn-danger btn-sm"
                                        @click="responseUpgradeRequest()"
                                      >
                                        Confirm
                                      </button>
                                    </div>
                                    <!-- End of Upgrade Level Status -->

                                    <!-- Start of Upgrade Level Button -->
                                    <!-- NOTE: CTA for reviewer -->
                                    <div
                                      v-if="
                                        (request_upgrade?.reviewer_id == uid ||
                                          role == 'Administrator' ||
                                          role == 'TPR') &&
                                        request_upgrade?.status == 2
                                      "
                                    >
                                      <button
                                        type="button"
                                        class="btn btn-danger btn-sm me-3"
                                        @click="upgradeReject()"
                                      >
                                        Reject Upgrade
                                      </button>
                                      <button
                                        type="button"
                                        class="btn btn-success btn-sm"
                                        @click="upgradeApprove()"
                                      >
                                        Approve Upgrade
                                      </button>
                                    </div>

                                    <!-- NOTE: CTA for requester -->
                                    <button
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        (request_upgrade?.status == 1 ||
                                          request_upgrade == null)
                                      "
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      data-bs-toggle="modal"
                                      data-bs-target="#notifyUpgrade"
                                    >
                                      Request to Upgrade Level
                                    </button>
                                    <!-- End of Upgrade Level Button -->
                                  </div>
                                </div>
                              </form>
                            </div>
                          </div>
                          <!--begin::Step 3-->

                          <!--begin::Step 2-->
                          <div v-else-if="sales_detail.level == 2">
                            <div
                              class="flex-column current"
                              data-kt-stepper-element="content"
                            >
                              <form>
                                <div class="row">
                                  <!-- Attachment of Customer Approval (SOW Signed / Proposal Approved) -->
                                  <div class="col-lg-6 mt-3">
                                    <h3>
                                      Attachment of Customer Approval (SOW
                                      Signed / Proposal Approved)
                                    </h3>
                                    <p class="text-danger">
                                      <small>* File size max 5MB</small>
                                    </p>
                                  </div>
                                  <div class="col-lg-6 mt-3 mb-20">
                                    <div
                                      class="position-relative"
                                      v-if="sales_detail"
                                    >
                                      <div
                                        class="position-absolute top-0 end-0"
                                        v-if="
                                          sales_detail.level == 2 &&
                                          sales_detail.status === 'Open'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-primary btn-sm"
                                          data-bs-toggle="modal"
                                          data-bs-target="#addFile"
                                          @click="addFile6()"
                                          v-if="
                                            role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          Upload Document
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="rounded box-d" id="myBorder">
                                    <div class="mt-3">
                                      <table class="table" v-if="files">
                                        <tr
                                          v-for="files in level2[0].data"
                                          :key="files.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="files.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  files.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                uploaded by
                                                <a href="#">{{
                                                  files.uploader_name
                                                }}</a>
                                              </p>
                                            </div>
                                          </td>
                                          <td
                                            class="d-flex justify-content-end"
                                            v-if="
                                              sales_detail.level == 2 &&
                                              sales_detail.status === 'Open'
                                            "
                                          >
                                            <button
                                              type="button"
                                              class="btn btn-danger btn-sm"
                                              @click="removeFile(files.id)"
                                              v-if="
                                                (role == 'AMS' ||
                                                  role == 'AM' ||
                                                  role == 'Administrator') &&
                                                (request_upgrade == null ||
                                                  request_upgrade?.status == 1)
                                              "
                                            >
                                              <span class="fas fa-trash"></span>
                                            </button>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>

                                  <div
                                    v-if="
                                      sales_detail.status === 'Open' &&
                                      sales_detail.level == 2 &&
                                      sales_detail.upgrade == true
                                    "
                                    class="text-center mt-10"
                                  >
                                    <!-- Start of Upgrade Level Status -->
                                    <!-- NOTE: Info for users other than requester & reviewer -->
                                    <div
                                      v-if="
                                        (role == 'TP' ||
                                          role == 'TPC' ||
                                          role == 'CBO') &&
                                        request_upgrade?.reviewer_id != uid &&
                                        request_upgrade?.status == 2
                                      "
                                      class="alert alert-warning fw-bold"
                                      role="alert"
                                      style="color: #e6b005"
                                    >
                                      This Sales Plan is under review by
                                      {{ request_upgrade?.reviewer_name }} to
                                      upgrade its level
                                    </div>

                                    <!-- NOTE: Info for other users than requester (AMS) -->
                                    <div
                                      v-if="
                                        (role == 'TP' ||
                                          role == 'TPC' ||
                                          role == 'CBO' ||
                                          role == 'TPR' ||
                                          role == 'Administrator') &&
                                        (request_upgrade?.status == 1 ||
                                          request_upgrade?.status == 4 ||
                                          request_upgrade == null)
                                      "
                                      class="alert alert-info fw-bold"
                                      role="alert"
                                    >
                                      No request created yet by AMS to upgrade
                                      the Sales Plan level
                                    </div>

                                    <!-- NOTE: Info for requester (Waiting) -->
                                    <div
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_upgrade?.status == 2
                                      "
                                      class="alert alert-info fw-bold"
                                      role="alert"
                                    >
                                      Sales Plan upgrade level requested,
                                      waiting for approval
                                    </div>

                                    <!-- NOTE: Info for requester (Rejected) -->
                                    <div
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        request_upgrade?.status == 4
                                      "
                                      class="alert alert-danger d-flex align-items-center justify-content-between"
                                      role="alert"
                                    >
                                      <span
                                        class="fas fa-triangle-exclamation text-danger display-6"
                                      ></span>
                                      <span>
                                        Your upgrade level request has been
                                        <strong>rejected</strong> with reason
                                        <strong
                                          >"{{
                                            request_upgrade?.reject_reason ??
                                            '-'
                                          }}"</strong
                                        >
                                      </span>
                                      <button
                                        type="button"
                                        class="btn btn-danger btn-sm"
                                        @click="responseUpgradeRequest()"
                                      >
                                        Confirm
                                      </button>
                                    </div>
                                    <!-- End of Upgrade Level Status -->

                                    <!-- Start of Upgrade Level Button -->
                                    <!-- NOTE: CTA for reviewer -->
                                    <div
                                      v-if="
                                        (request_upgrade?.reviewer_id == uid ||
                                          role == 'Administrator' ||
                                          role == 'TPR') &&
                                        request_upgrade?.status == 2
                                      "
                                    >
                                      <button
                                        type="button"
                                        class="btn btn-danger btn-sm me-3"
                                        @click="upgradeReject()"
                                      >
                                        Reject Upgrade
                                      </button>
                                      <button
                                        type="button"
                                        class="btn btn-success btn-sm"
                                        @click="upgradeApprove()"
                                      >
                                        Approve Upgrade
                                      </button>
                                    </div>

                                    <!-- NOTE: CTA for requester -->
                                    <button
                                      v-if="
                                        (role == 'AMS' || role == 'AM') &&
                                        (request_upgrade?.status == 1 ||
                                          request_upgrade == null)
                                      "
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      data-bs-toggle="modal"
                                      data-bs-target="#notifyUpgrade"
                                    >
                                      Request to Upgrade Level
                                    </button>
                                    <!-- End of Upgrade Level Button -->
                                  </div>
                                </div>
                              </form>
                            </div>
                          </div>
                          <!--begin::Step 2-->

                          <!--begin::Step 1-->
                          <div v-else-if="sales_detail.level == 1">
                            <div
                              class="flex-column current"
                              data-kt-stepper-element="content"
                            >
                              <div class="row">
                                <!-- Attachment of WO/PO number customer document -->
                                <div class="col-lg-6 mt-3">
                                  <h3>
                                    Attachment of WO/PO Number Customer Document
                                  </h3>
                                </div>
                                <div class="col-lg-6 mt-3 mb-20">
                                  <div class="position-relative">
                                    <div class="position-absolute top-0 end-0">
                                      <button
                                        v-if="
                                          sales_detail.status == 'Open' &&
                                          (role == 'AMS' ||
                                            role == 'AM' ||
                                            role == 'Administrator') &&
                                          (request_closed == null ||
                                            request_closed?.status == 1)
                                        "
                                        type="button"
                                        class="btn btn-primary btn-sm"
                                        data-bs-toggle="modal"
                                        data-bs-target="#addFile"
                                        @click="addFile7()"
                                      >
                                        Upload Document
                                      </button>
                                    </div>
                                  </div>
                                </div>
                                <div class="rounded box-d" id="myBorder">
                                  <div class="mt-3">
                                    <table class="table" v-if="files">
                                      <tr
                                        v-for="files in level1[0].data"
                                        :key="files.id"
                                      >
                                        <td>
                                          <div
                                            class="d-flex justify-content-start align-items-center"
                                          >
                                            <a
                                              :href="files.full_path"
                                              class="btn btn-outline-primary btn-outline btn-sm mb-2"
                                              target="_blank"
                                            >
                                              <strong>{{
                                                files.file_name
                                              }}</strong>
                                            </a>
                                            <p class="text-muted ms-3">
                                              uploaded by
                                              <a href="#">{{
                                                files.uploader_name
                                              }}</a>
                                            </p>
                                          </div>
                                        </td>
                                        <td class="d-flex justify-content-end">
                                          <button
                                            type="button"
                                            class="btn btn-danger btn-sm"
                                            @click="removeFile(files.id)"
                                            v-if="
                                              sales_detail.status == 'Open' &&
                                              (role == 'AMS' ||
                                                role == 'AM' ||
                                                role == 'Administrator') &&
                                              (request_closed == null ||
                                                request_closed?.status == 1)
                                            "
                                          >
                                            <span class="fas fa-trash"></span>
                                          </button>
                                        </td>
                                      </tr>
                                    </table>
                                  </div>
                                </div>

                                <div
                                  v-if="
                                    sales_detail.status === 'Open' &&
                                    sales_detail.level === 1
                                  "
                                  class="text-center mt-5"
                                >
                                  <!-- Start of Upgrade Level Status -->
                                  <!-- NOTE: Info for users other than requester & reviewer -->
                                  <div
                                    v-if="
                                      (role == 'TP' ||
                                        role == 'TPC' ||
                                        role == 'CBO') &&
                                      request_closed?.reviewer_id != uid &&
                                      request_closed?.status == 2
                                    "
                                    class="alert alert-warning fw-bold"
                                    role="alert"
                                    style="color: #e6b005"
                                  >
                                    This Sales Plan is under review by
                                    {{ request_upgrade?.reviewer_name }} to be
                                    Closed Sales
                                  </div>

                                  <!-- NOTE: Info for other users than requester (AMS) -->
                                  <div
                                    v-if="
                                      (role == 'TP' ||
                                        role == 'TPC' ||
                                        role == 'CBO' ||
                                        role == 'TPR' ||
                                        role == 'Administrator') &&
                                      (request_closed?.status == 1 ||
                                        request_closed?.status == 4 ||
                                        request_closed == null)
                                    "
                                    class="alert alert-info fw-bold"
                                    role="alert"
                                  >
                                    No request created yet by AMS to Closed
                                    Sales
                                  </div>

                                  <!-- NOTE: Info for requester (Waiting) -->
                                  <div
                                    v-if="
                                      (role == 'AMS' || role == 'AM') &&
                                      request_closed?.status == 2
                                    "
                                    class="alert alert-info fw-bold"
                                    role="alert"
                                  >
                                    Closed Sales requested, waiting for approval
                                  </div>

                                  <!-- NOTE: Info for requester (Rejected) -->
                                  <div
                                    v-if="
                                      (role == 'AMS' || role == 'AM') &&
                                      request_closed?.status == 4
                                    "
                                    class="alert alert-danger d-flex align-items-center justify-content-between"
                                    role="alert"
                                  >
                                    <span
                                      class="fas fa-triangle-exclamation text-danger display-6"
                                    ></span>
                                    <span>
                                      Your closed sales request has been
                                      <strong>rejected</strong> with reason
                                      <strong
                                        >"{{
                                          request_closed?.reject_reason ?? '-'
                                        }}"</strong
                                      >
                                    </span>
                                    <button
                                      type="button"
                                      class="btn btn-danger btn-sm"
                                      @click="responseClosedSalesRequest()"
                                    >
                                      Confirm
                                    </button>
                                  </div>
                                  <!-- End of Upgrade Level Status -->

                                  <!-- Start of Upgrade Level Button -->
                                  <!-- NOTE: CTA for reviewer -->
                                  <div
                                    v-if="
                                      (request_closed?.reviewer_id == uid ||
                                        role == 'Administrator' ||
                                        role == 'TPR') &&
                                      request_closed?.status == 2
                                    "
                                  >
                                    <button
                                      type="button"
                                      class="btn btn-danger btn-sm me-3"
                                      @click="closedSalesReject()"
                                    >
                                      Reject Closed Sales
                                    </button>
                                    <button
                                      type="button"
                                      class="btn btn-success btn-sm"
                                      @click="closedSalesApprove()"
                                    >
                                      Approve Closed Sales
                                    </button>
                                  </div>

                                  <!-- NOTE: CTA for requester -->
                                  <button
                                    v-if="
                                      (role == 'AMS' || role == 'AM') &&
                                      level1[0].data.length >= 1 &&
                                      (request_closed?.status == 1 ||
                                        request_closed == null)
                                    "
                                    type="button"
                                    class="btn btn-info btn-sm"
                                    data-bs-toggle="modal"
                                    data-bs-target="#requestClosedSales"
                                  >
                                    Request to Closed Sales
                                  </button>
                                  <!-- End of Upgrade Level Button -->
                                </div>

                                <!-- Input SO Number -->
                                <div class="col-lg-6 mt-8 mb-3">
                                  <h3>Input SO Number</h3>
                                </div>
                                <div class="row">
                                  <!-- NOTE: SO Number form for Admin & CBO -->
                                  <div
                                    v-if="
                                      role == 'Administrator' ||
                                      role == 'AMS' ||
                                      role == 'AM'
                                    "
                                    class="input-group mb-5"
                                  >
                                    <input
                                      type="text"
                                      class="form-control"
                                      v-model="level1[1].data"
                                      :class="
                                        sales_detail.status != 'Closed Sales'
                                          ? 'bg-secondary'
                                          : ''
                                      "
                                      :readonly="
                                        sales_detail.status != 'Closed Sales'
                                      "
                                    />
                                    <button
                                      class="btn btn-info btn-sm fw-bold"
                                      type="button"
                                      @click="inputSoNumber()"
                                      :disabled="
                                        sales_detail.status != 'Closed Sales'
                                      "
                                      :class="
                                        sales_detail.status == 'Closed In'
                                          ? 'd-none'
                                          : ''
                                      "
                                    >
                                      Save SO Number
                                    </button>
                                  </div>

                                  <!-- NOTE: Readonly input for users other than Admin & CBO -->
                                  <input
                                    v-else
                                    type="text"
                                    class="form-control bg-secondary mb-5"
                                    v-model="level1[1].data"
                                    readonly
                                  />
                                </div>
                              </div>
                            </div>
                          </div>
                          <!--begin::Step 1-->
                        </div>

                        <!-- NOTE: in case Sales Plan has been locked (closed-in/cancelled) -->
                        <div v-else>
                          <div
                            role="alert"
                            class="alert text-center"
                            :class="
                              sales_detail.status == 'Cancel'
                                ? 'alert-danger'
                                : 'alert-primary'
                            "
                          >
                            <span
                              class="h1 align-middle"
                              :class="
                                sales_detail.status == 'Cancel'
                                  ? 'text-danger'
                                  : 'text-primary'
                              "
                            >
                              This Sales Plan has been
                              {{
                                sales_detail.status == 'Cancel'
                                  ? 'cancelled'
                                  : 'closed'
                              }}
                            </span>
                            <button
                              v-if="
                                sales_reject && sales_detail.status == 'Cancel'
                              "
                              type="button"
                              class="btn btn-sm btn-danger ms-3"
                              @click="showDetailCancel()"
                            >
                              See detail
                            </button>
                          </div>
                        </div>
                      </div>
                      <!--end::Group-->
                    </div>
                    <!--end::Form-->
                  </div>
                  <!--end::Stepper-->
                </div>

                <!-- Tab History -->
                <div
                  class="tab-pane fade"
                  id="history-tab-pane"
                  role="tabpanel"
                  aria-labelledby="history-tab"
                  tabindex="0"
                >
                  <!-- Tab Hisotry Detail -->
                  <div class="row mt-5">
                    <hr />
                    <ul
                      class="nav nav-pills nav-fill"
                      id="pills-tab"
                      role="tablist"
                    >
                      <li class="nav-item" role="presentation">
                        <button
                          class="nav-link active lead"
                          id="history-file-tab"
                          data-bs-toggle="tab"
                          data-bs-target="#history-file-tab-pane"
                          type="button"
                          role="tab"
                          aria-controls="history-file-tab-pane"
                          aria-selected="true"
                        >
                          History File
                        </button>
                      </li>
                      <li class="nav-item" role="presentation">
                        <button
                          class="nav-link"
                          id="history-activity-tab"
                          data-bs-toggle="tab"
                          data-bs-target="#history-activity-tab-pane"
                          type="button"
                          role="tab"
                          aria-controls="history-activity-tab-pane"
                          aria-selected="false"
                        >
                          History Activity
                        </button>
                      </li>
                    </ul>

                    <div class="tab-content" id="pills-tabContent">
                      <!-- Tab Hisotry File -->
                      <div
                        class="tab-pane fade show active"
                        id="history-file-tab-pane"
                        role="tabpanel"
                        aria-labelledby="history-file-tab"
                        tabindex="0"
                      >
                        <div class="mt-5">
                          <!-- Filter Month -->
                          <div class="row">
                            <div class="col-lg-8"></div>
                            <div
                              class="col-lg-4 col-sm-12 d-flex justify-content-end align-items-center"
                            >
                              <select class="form-select" v-model="filter">
                                <option :value="null" disabled>
                                  Select Month
                                </option>
                                <option value="1">January</option>
                                <option value="2">February</option>
                                <option value="3">March</option>
                                <option value="4">April</option>
                                <option value="5">May</option>
                                <option value="6">June</option>
                                <option value="7">July</option>
                                <option value="8">August</option>
                                <option value="9">September</option>
                                <option value="10">October</option>
                                <option value="11">November</option>
                                <option value="12">December</option>
                              </select>
                              <button
                                type="button"
                                class="btn btn-info btn-sm ms-3"
                                @click="
                                  listFileHistory()
                                  clearFormHistory()
                                "
                              >
                                Filter
                              </button>
                            </div>
                          </div>

                          <!-- File History Level 4 -->
                          <h2>Sales Level 4</h2>
                          <div class="rounded box-d p-5 my-3" id="myBorder2">
                            <!-- Check if data exists and contains history.level4 -->
                            <div
                              v-if="
                                file_histories &&
                                file_histories.level4 &&
                                file_histories.level4.length > 0
                              "
                            >
                              <!-- Loop through level4 files -->
                              <div
                                v-for="(
                                  history, index
                                ) in file_histories.level4"
                                :key="index"
                              >
                                <div class="row">
                                  <div class="col-lg-6 col-sm-12">
                                    <h5>
                                      <span class="fas fa-calendar-days"></span>
                                      {{ history.uploadedAt }}
                                    </h5>
                                  </div>
                                </div>
                                <div class="row mt-2">
                                  <h5>
                                    {{ history.totalFiles }} File Uploaded
                                  </h5>
                                  <div class="rounded box-d mb-5" id="myBorder">
                                    <div class="mt-2">
                                      <table class="table">
                                        <tr
                                          v-for="(
                                            uploadedFile, index
                                          ) in history.files"
                                          :key="uploadedFile.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="uploadedFile.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  uploadedFile.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                <small
                                                  >by
                                                  <a href="#">{{
                                                    history.files[index]
                                                      .uploader_name
                                                  }}</a></small
                                                >
                                              </p>
                                            </div>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else>
                              <p>No data available.</p>
                            </div>
                          </div>

                          <!-- File History Level 3 -->
                          <h2>Sales Level 3</h2>
                          <div class="rounded box-d p-5 my-3" id="myBorder2">
                            <!-- Check if data exists and contains history.level3 -->
                            <div
                              v-if="
                                file_histories &&
                                file_histories.level3 &&
                                file_histories.level3.length > 0
                              "
                            >
                              <!-- Loop through level3 files -->
                              <div
                                v-for="(
                                  history, index
                                ) in file_histories.level3"
                                :key="index"
                              >
                                <div class="row">
                                  <div class="col-lg-6 col-sm-12">
                                    <h5>
                                      <span class="fas fa-calendar-days"></span>
                                      {{ history.uploadedAt }}
                                    </h5>
                                  </div>
                                </div>
                                <div class="row mt-2">
                                  <h5>
                                    {{ history.totalFiles }} File Uploaded
                                  </h5>
                                  <div class="rounded box-d mb-5" id="myBorder">
                                    <div class="mt-2">
                                      <table class="table">
                                        <tr
                                          v-for="(
                                            uploadedFile, index
                                          ) in history.files"
                                          :key="uploadedFile.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="uploadedFile.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  uploadedFile.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                <small
                                                  >by
                                                  <a href="#">{{
                                                    history.files[index]
                                                      .uploader_name
                                                  }}</a></small
                                                >
                                              </p>
                                            </div>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else>
                              <p>No data available.</p>
                            </div>
                          </div>

                          <!-- File History Level 2 -->
                          <h2>Sales Level 2</h2>
                          <div class="rounded box-d p-5 my-3" id="myBorder2">
                            <!-- Check if data exists and contains history.level2 -->
                            <div
                              v-if="
                                file_histories &&
                                file_histories.level2 &&
                                file_histories.level2.length > 0
                              "
                            >
                              <!-- Loop through level2 files -->
                              <div
                                v-for="(
                                  history, index
                                ) in file_histories.level2"
                                :key="index"
                              >
                                <div class="row">
                                  <div class="col-lg-6 col-sm-12">
                                    <h5>
                                      <span class="fas fa-calendar-days"></span>
                                      {{ history.uploadedAt }}
                                    </h5>
                                  </div>
                                </div>
                                <div class="row mt-2">
                                  <h5>
                                    {{ history.totalFiles }} File Uploaded
                                  </h5>
                                  <div class="rounded box-d mb-5" id="myBorder">
                                    <div>
                                      <table class="table">
                                        <tr
                                          v-for="(
                                            uploadedFile, index
                                          ) in history.files"
                                          :key="uploadedFile.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="uploadedFile.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  uploadedFile.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                <small
                                                  >by
                                                  <a href="#">{{
                                                    history.files[index]
                                                      .uploader_name
                                                  }}</a></small
                                                >
                                              </p>
                                            </div>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else>
                              <p>No data available.</p>
                            </div>
                          </div>

                          <!-- File History Level 1 -->
                          <h2>Sales Level 1</h2>
                          <div class="rounded box-d p-5 my-3" id="myBorder2">
                            <!-- Check if data exists and contains history.level1 -->
                            <div
                              v-if="
                                file_histories &&
                                file_histories.level1 &&
                                file_histories.level1.length > 0
                              "
                            >
                              <!-- Loop through level1 files -->
                              <div
                                v-for="(
                                  history, index
                                ) in file_histories.level1"
                                :key="index"
                              >
                                <div class="row">
                                  <div class="col-lg-6 col-sm-12">
                                    <h5>
                                      <span class="fas fa-calendar-days"></span>
                                      {{ history.uploadedAt }}
                                    </h5>
                                  </div>
                                </div>
                                <div class="row mt-2">
                                  <h5>
                                    {{ history.totalFiles }} File Uploaded
                                  </h5>
                                  <div class="rounded box-d mb-5" id="myBorder">
                                    <div class="mt-2">
                                      <table class="table">
                                        <tr
                                          v-for="(
                                            uploadedFile, index
                                          ) in history.files"
                                          :key="uploadedFile.id"
                                        >
                                          <td>
                                            <div
                                              class="d-flex justify-content-start align-items-center"
                                            >
                                              <a
                                                :href="uploadedFile.full_path"
                                                class="btn btn-outline-primary btn-outline btn-sm"
                                                target="_blank"
                                              >
                                                <strong>{{
                                                  uploadedFile.file_name
                                                }}</strong>
                                              </a>
                                              <p class="text-muted ms-3">
                                                <small
                                                  >by
                                                  <a href="#">{{
                                                    history.files[index]
                                                      .uploader_name
                                                  }}</a></small
                                                >
                                              </p>
                                            </div>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else>
                              <p>No data available.</p>
                            </div>
                          </div>
                        </div>
                      </div>
                      <!-- Tab Hisotry Activity -->
                      <div
                        class="tab-pane fade border rounded p-3 mt-3"
                        id="history-activity-tab-pane"
                        role="tabpanel"
                        aria-labelledby="history-activity-tab"
                        tabindex="0"
                      >
                        <div
                          class="accordion accordion-flush my-3"
                          :id="'accordionFlush' + log.id"
                          v-if="sales_history.length > 0"
                          v-for="log in sales_history"
                          :key="log.id"
                        >
                          <div class="accordion-item">
                            <h2
                              class="accordion-header"
                              :id="'flush-headingOne' + log.id"
                            >
                              <button
                                class="accordion-button collapsed btn btn-secondary"
                                type="button"
                                data-bs-toggle="collapse"
                                :data-bs-target="'#flush-collapseOne' + log.id"
                                aria-expanded="false"
                                :aria-controls="'flush-collapseOne' + log.id"
                              >
                                {{ log.description }} - ({{
                                  log.created_at
                                    | formatDate
                                    | changeDateFormat
                                }})
                              </button>
                            </h2>
                            <div
                              :id="'flush-collapseOne' + log.id"
                              class="accordion-collapse collapse"
                              :aria-labelledby="'flush-headingOne' + log.id"
                              :data-bs-parent="'#accordionFlush' + log.id"
                            >
                              <div class="accordion-body">
                                <div class="row mt-2">
                                  <div class="col">
                                    <div class="border rounded p-3">
                                      <h5>Old Values</h5>
                                      <table class="table">
                                        <tbody v-if="log.properties.old_values">
                                          <tr>
                                            <td>Sales Plan</td>
                                            <td>:</td>
                                            <td>
                                              USD
                                              {{
                                                formatPrice(
                                                  log.properties.old_values
                                                    ?.value
                                                )
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Registration</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.old_values
                                                  ?.ac_reg
                                                  ? log.properties.old_values
                                                      ?.ac_reg
                                                  : '-'
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Maintenance</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.old_values
                                                  ?.maintenance_name
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Product</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.old_values
                                                  ?.product_name
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>TAT</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.old_values?.tat
                                              }}
                                              Days
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>AMS</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.old_values
                                                  ?.ams_initial
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Start Date</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.old_values
                                                  ?.start_date
                                                  | formatDate
                                                  | changeDateFormat
                                              }}
                                            </td>
                                          </tr>
                                        </tbody>
                                        <tbody v-else>
                                          <tr>
                                            <td colspan="3">
                                              No old values available
                                            </td>
                                          </tr>
                                        </tbody>
                                      </table>
                                    </div>
                                  </div>
                                  <div class="col">
                                    <div class="border rounded p-3">
                                      <h5>New Values</h5>
                                      <table class="table">
                                        <tbody v-if="log.properties.new_values">
                                          <tr>
                                            <td>Sales Plan</td>
                                            <td>:</td>
                                            <td>
                                              USD
                                              {{
                                                formatPrice(
                                                  log.properties.new_values
                                                    ?.value
                                                )
                                                  ? log.properties.new_values
                                                      ?.value
                                                  : log.properties.old_values
                                                      ?.value
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Registration</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.new_values
                                                  ?.ac_reg
                                                  ? log.properties.new_values
                                                      ?.ac_reg
                                                  : log.properties.old_values
                                                      ?.ac_reg
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Maintenance</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.new_values
                                                  ?.maintenance_name
                                                  ? log.properties.new_values
                                                      ?.maintenance_name
                                                  : log.properties.old_values
                                                      ?.maintenance_name
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Product</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.new_values
                                                  ?.product_name
                                                  ? log.properties.new_values
                                                      ?.product_name
                                                  : log.properties.old_values
                                                      ?.product_name
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>TAT</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.new_values?.tat
                                                  ? log.properties.new_values
                                                      ?.tat
                                                  : log.properties.old_values
                                                      ?.tat
                                              }}
                                              Days
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>AMS</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.new_values
                                                  ?.ams_initial
                                                  ? log.properties.new_values
                                                      ?.ams_initial
                                                  : log.properties.old_values
                                                      ?.ams_initial
                                              }}
                                            </td>
                                          </tr>
                                          <tr>
                                            <td>Start Date</td>
                                            <td>:</td>
                                            <td>
                                              {{
                                                log.properties.new_values
                                                  ?.start_date
                                                  | formatDate
                                                  | changeDateFormat
                                              }}
                                            </td>
                                          </tr>
                                        </tbody>
                                        <tbody v-else>
                                          <tr>
                                            <td colspan="3">
                                              No new values available
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
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Tab Contact -->
                <div
                  class="tab-pane fade"
                  id="contact-tab-pane"
                  role="tabpanel"
                  aria-labelledby="contact-tab"
                  tabindex="0"
                >
                  <div class="mt-5" v-if="sales_detail">
                    <div class="table-responsive">
                      <table
                        class="table table-row-bordered table-row-gray-200 gy-4"
                        v-if="contact"
                      >
                        <thead>
                          <tr class="fw-bold fs-6 text-gray-800">
                            <th>No</th>
                            <th>Name</th>
                            <th>Phone</th>
                            <th>Email</th>
                            <th>Title</th>
                            <th
                              v-if="
                                role == 'AMS' ||
                                role == 'AM' ||
                                role == 'Administrator' ||
                                role == 'TPR'
                              "
                            >
                              Action
                            </th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-if="level4[0].data.length === 0">
                            <td
                              colspan="6"
                              class="text-muted text-center pt-10"
                            >
                              This Customer has no contact persons yet.
                            </td>
                          </tr>
                          <tr
                            v-if="level4[0].data.length !== 0"
                            v-for="(contact, contact_index) in level4[0].data"
                            @dblclick="getItem(contact_index)"
                            :key="contact_index"
                          >
                            <td>
                              {{ contact_index + 1 }}
                            </td>
                            <td>
                              {{ contact.name }}
                            </td>
                            <td>
                              {{ contact.phone }}
                            </td>
                            <td>
                              {{ contact.email }}
                            </td>
                            <td>
                              {{ contact.title }}
                            </td>
                            <td
                              v-if="
                                role == 'AMS' ||
                                role == 'AM' ||
                                role == 'Administrator' ||
                                role == 'TPR'
                              "
                            >
                              <button
                                class="btn btn-sm btn-light text-center"
                                @click="removeContact(contact.id)"
                              >
                                <i class="bi bi-trash-fill text-danger"></i>
                              </button>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </div>

                    <div
                      class="text-center mt-10"
                      v-if="
                        sales_detail.status === 'Open' ||
                        sales_detail.status === 'Closed Sales'
                      "
                    >
                      <button
                        type="button"
                        class="btn btn-primary btn-sm"
                        data-bs-toggle="modal"
                        data-bs-target="#addContact"
                        @click="addContact()"
                      >
                        Add Contact Person
                      </button>
                    </div>
                  </div>
                </div>

                <!-- Tab Reschedule -->
                <div
                  class="tab-pane fade"
                  id="reschedule-tab-pane"
                  role="tabpanel"
                  aria-labelledby="reschedule-tab"
                  tabindex="0"
                >
                  <div class="mt-5" v-if="sales_detail">
                    <form>
                      <!-- NOTE: Display-only | in case request has been sent and under review -->
                      <div
                        v-if="
                          sales_reschedule &&
                          request_reschedule &&
                          (request_reschedule?.status == 2 ||
                            request_reschedule?.status == 4)
                        "
                      >
                        <div
                          v-if="
                            request_reschedule?.status == 2 &&
                            sales_reschedule?.sameYear == false
                          "
                          class="row mb-3"
                        >
                          <div class="text-center">
                            <div
                              class="alert alert-danger alert-dismissible fade show d-flex align-items-center justify-content-between"
                              role="alert"
                            >
                              <span
                                class="fas fa-triangle-exclamation text-danger display-6"
                              ></span>
                              <span>
                                This Sales Plan potentially to be
                                <b>cancelled</b> due the reschedule requested to
                                <strong>different year</strong> with the current
                                Sales Plan schedule,<br />instead the System
                                will automatically
                                <b>create new Sales Plan data</b> that is same
                                as this current one with different schedule.
                              </span>
                              <button
                                type="button"
                                class="btn btn-danger btn-sm"
                                data-bs-dismiss="alert"
                              >
                                Close
                              </button>
                            </div>
                          </div>
                        </div>

                        <div v-if="sales_reschedule.hangar != null" class="row">
                          <div class="col-lg-12">
                            <div class="mb-3">
                              <label class="form-label">Hangar</label>
                              <input
                                type="text"
                                class="form-control"
                                disabled
                                :value="sales_reschedule?.hangar.name"
                              />
                            </div>
                          </div>
                        </div>

                        <div class="row">
                          <div class="col-lg-6">
                            <div class="mb-3">
                              <label class="form-label"
                                >Reschedule to (Start Date)</label
                              >
                              <input
                                type="text"
                                class="form-control"
                                disabled
                                :value="sales_reschedule?.startDate"
                              />
                            </div>
                          </div>
                          <div class="col-lg-6">
                            <div class="mb-3">
                              <label class="form-label">TAT</label>
                              <input
                                type="text"
                                class="form-control"
                                disabled
                                :value="sales_reschedule?.tat + ' Days'"
                              />
                            </div>
                          </div>
                        </div>

                        <div
                          v-if="role == 'AMS' || role == 'AM'"
                          class="row mt-5"
                        >
                          <div
                            v-if="request_reschedule?.status == 4"
                            class="text-center"
                          >
                            <div
                              class="alert alert-danger d-flex align-items-center justify-content-between"
                              role="alert"
                            >
                              <span
                                class="fas fa-triangle-exclamation text-danger display-6"
                              ></span>
                              <span>
                                Your reschedule request has been
                                <strong>rejected</strong> with reason
                                <strong
                                  >"{{
                                    request_reschedule?.reject_reason ?? '-'
                                  }}"</strong
                                >
                              </span>
                              <button
                                type="button"
                                class="btn btn-danger btn-sm"
                                @click="responseRescheduleRequest()"
                              >
                                Confirm
                              </button>
                            </div>
                          </div>

                          <div v-else class="text-center">
                            <div class="alert alert-info fw-bold" role="alert">
                              Reschedule requested to
                              {{ request_reschedule?.reviewer_name }}, waiting
                              for approval
                            </div>
                          </div>
                        </div>

                        <div v-else class="row mt-5">
                          <div
                            v-if="
                              request_reschedule?.reviewer_id == uid ||
                              role == 'Administrator' ||
                              role == 'TPR'
                            "
                            class="text-center"
                          >
                            <div v-if="request_reschedule?.status == 4">
                              <div
                                class="alert alert-info fw-bold"
                                role="alert"
                              >
                                Your rejection response has been sent, waiting
                                for confirmation
                              </div>
                            </div>
                            <div v-else>
                              <button
                                type="button"
                                class="btn btn-danger btn-sm"
                                @click="salesRescheduleReject()"
                              >
                                Reject Reschedule
                              </button>
                              <button
                                type="button"
                                class="btn btn-success btn-sm mx-2"
                                @click="salesRescheduleApprove()"
                              >
                                Approve Reschedule
                              </button>
                            </div>
                          </div>

                          <div v-else class="text-center">
                            <div
                              class="alert alert-warning fw-bold"
                              role="alert"
                              style="color: #e6b005"
                            >
                              This Sales Plane is under review by
                              {{ request_reschedule?.reviewer_name }} to be
                              rescheduled
                            </div>
                          </div>
                        </div>
                      </div>

                      <!-- NOTE: Reschedule form to send -->
                      <div v-else>
                        <div v-if="role == 'AMS' || role == 'AM'">
                          <div class="row">
                            <div class="col-12">
                              <div class="mb-3">
                                <label class="form-label"
                                  >Request to
                                  <span class="text-danger">*</span></label
                                >
                                <select
                                  v-if="
                                    sales_detail.product.name == 'Airframe' ||
                                    sales_detail.product.name == 'Line'
                                  "
                                  v-model="user_id"
                                  class="form-select"
                                >
                                  <option :value="null" disabled>
                                    Select User
                                  </option>
                                  <option
                                    v-for="(
                                      user_options, user_index
                                    ) in user_option"
                                    v-if="user_options.role_id === 5"
                                    :value="user_options.id"
                                    :key="user_index"
                                  >
                                    {{ user_options.name }} -
                                    {{ user_options.email }}
                                  </option>
                                </select>

                                <select
                                  v-else
                                  v-model="user_id"
                                  class="form-select"
                                >
                                  <option :value="null" disabled>
                                    Select User
                                  </option>
                                  <option
                                    v-for="(
                                      user_options, user_index
                                    ) in user_option"
                                    v-if="
                                      user_options.role_id === 1 ||
                                      user_options.role_id === 4
                                    "
                                    :value="user_options.id"
                                    :key="user_index"
                                  >
                                    {{ user_options.name }} -
                                    {{ user_options.email }}
                                  </option>
                                </select>
                              </div>
                            </div>
                          </div>

                          <div
                            v-if="
                              level4[3].data != null &&
                              request_hangar?.status != 2
                            "
                            class="row"
                          >
                            <div class="col-lg-12">
                              <div class="mb-3">
                                <label class="form-label">Hangar</label>
                                <select v-model="hangar_id" class="form-select">
                                  <option :value="null" disabled>
                                    Select Hangar
                                  </option>
                                  <option
                                    v-for="(
                                      hangar_options, hangar_index
                                    ) in hangar_option"
                                    :value="hangar_options.id"
                                    :key="hangar_index"
                                  >
                                    {{ hangar_options.name }}
                                  </option>
                                </select>
                              </div>
                            </div>
                          </div>

                          <div class="row">
                            <div class="col-lg-6">
                              <div class="mb-3">
                                <label class="form-label"
                                  >Reschedule to (Start Date)
                                  <span class="text-danger">*</span></label
                                >
                                <input
                                  type="date"
                                  class="form-control"
                                  v-model="reschedule_date"
                                />
                              </div>
                            </div>
                            <div class="col-lg-6">
                              <div class="mb-3">
                                <label class="form-label"
                                  >TAT <span class="text-danger">*</span></label
                                >
                                <input
                                  type="number"
                                  class="form-control"
                                  v-model="reschedule_tat"
                                />
                              </div>
                            </div>
                          </div>
                          <div class="row mt-5">
                            <div class="text-center">
                              <button
                                v-if="
                                  request_reschedule == null ||
                                  request_reschedule?.status == 1
                                "
                                type="button"
                                class="btn btn-info btn-sm"
                                @click="salesReschedule()"
                              >
                                Request to Reschedule
                              </button>
                            </div>
                          </div>
                        </div>

                        <div v-else class="row text-center mt-10">
                          <div class="alert alert-info fw-bold" role="alert">
                            At this time, there is no reschedule request for
                            this Sales Plan
                          </div>
                        </div>
                      </div>
                    </form>
                  </div>
                </div>

                <!-- Tab Cancel -->
                <div
                  class="tab-pane fade"
                  id="cancel-tab-pane"
                  role="tabpanel"
                  aria-labeledby="cancel-tab"
                  tabindex="0"
                >
                  <div class="mt-5" v-if="sales_detail">
                    <form>
                      <!-- NOTE: Display-only | in case request has been sent and under review -->
                      <div
                        v-if="
                          sales_reject &&
                          request_cancel &&
                          (request_cancel?.status == 2 ||
                            request_cancel?.status == 4)
                        "
                      >
                        <div class="row">
                          <div class="col-12">
                            <div class="mb-3">
                              <label class="form-label">Cancel Category</label>
                              <input
                                type="text"
                                class="form-control"
                                disabled
                                :value="sales_reject?.category"
                              />
                            </div>
                          </div>
                        </div>

                        <div class="row">
                          <div class="col-12">
                            <div class="mb-3">
                              <label class="form-label">Cancel Reason</label>
                              <textarea
                                class="form-control"
                                cols="30"
                                rows="5"
                                disabled
                                :value="sales_reject?.reason"
                              ></textarea>
                            </div>
                          </div>
                        </div>

                        <div
                          v-if="role == 'AMS' || role == 'AM'"
                          class="row mt-5"
                        >
                          <div
                            v-if="request_cancel?.status == 4"
                            class="text-center"
                          >
                            <div
                              class="alert alert-danger d-flex align-items-center justify-content-between"
                              role="alert"
                            >
                              <span
                                class="fas fa-triangle-exclamation text-danger display-6"
                              ></span>
                              <span>
                                Your cancel request has been
                                <strong>rejected</strong> with reason
                                <strong
                                  >"{{
                                    request_cancel?.reject_reason ?? '-'
                                  }}"</strong
                                >
                              </span>
                              <button
                                type="button"
                                class="btn btn-danger btn-sm"
                                @click="responseCancelRequest()"
                              >
                                Confirm
                              </button>
                            </div>
                          </div>

                          <div v-else class="text-center">
                            <div class="alert alert-info fw-bold" role="alert">
                              Cancel Sales requested to
                              {{ request_cancel?.reviewer_name }}, waiting for
                              approval
                            </div>
                          </div>
                        </div>

                        <div v-else class="row mt-5">
                          <div
                            v-if="
                              request_cancel?.reviewer_id == uid ||
                              role == 'Administrator' ||
                              role == 'TPR'
                            "
                            class="text-center"
                          >
                            <div v-if="request_cancel?.status == 4">
                              <div
                                class="alert alert-info fw-bold"
                                role="alert"
                              >
                                Your rejection response has been sent, waiting
                                for confirmation
                              </div>
                            </div>
                            <div v-else>
                              <button
                                type="button"
                                class="btn btn-danger btn-sm"
                                @click="salesCancelReject()"
                              >
                                Reject to Cancel Sales
                              </button>
                              <button
                                type="button"
                                class="btn btn-success btn-sm mx-2"
                                @click="salesCancelApprove()"
                              >
                                Approve to Cancel Sales
                              </button>
                            </div>
                          </div>

                          <div v-else class="text-center">
                            <div
                              class="alert alert-warning fw-bold"
                              role="alert"
                              style="color: #e6b005"
                            >
                              This Sales Plane is under review by
                              {{ request_cancel?.reviewer_name }} to be
                              cancelled
                            </div>
                          </div>
                        </div>
                      </div>

                      <!-- NOTE: Cancel form to send -->
                      <div v-else>
                        <div v-if="role == 'AMS' || role == 'AM'">
                          <div class="row">
                            <div class="col-12">
                              <div class="mb-3">
                                <label class="form-label"
                                  >Request to
                                  <span class="text-danger">*</span></label
                                >
                                <select v-model="user_id" class="form-select">
                                  <option :value="null" disabled>
                                    Select User
                                  </option>
                                  <option
                                    v-for="(
                                      user_options, user_index
                                    ) in user_option"
                                    v-if="
                                      user_options.role_id === 4 ||
                                      user_options.role_id === 1
                                    "
                                    :value="user_options.id"
                                    :key="user_index"
                                  >
                                    {{ user_options.name }} -
                                    {{ user_options.email }}
                                  </option>
                                </select>
                              </div>
                            </div>
                          </div>

                          <div class="row">
                            <div class="col-12">
                              <div class="mb-3">
                                <label class="form-label"
                                  >Cancel Category
                                  <span class="text-danger">*</span></label
                                >
                                <select
                                  v-model="cancel_category_id"
                                  class="form-select"
                                >
                                  <option :value="null" disabled>
                                    Select Category
                                  </option>
                                  <option
                                    v-for="(
                                      category_options, category_index
                                    ) in category_option"
                                    v-if="category_options.id != 2"
                                    :value="category_options.id"
                                    :key="category_index"
                                  >
                                    {{ category_options.name }}
                                  </option>
                                </select>
                              </div>
                            </div>
                          </div>

                          <div class="row">
                            <div class="col-12">
                              <div class="mb-3">
                                <label class="form-label"
                                  >Cancel Reason
                                  <span class="text-danger">*</span></label
                                >
                                <textarea
                                  class="form-control"
                                  cols="30"
                                  rows="5"
                                  v-model="cancel_reason"
                                ></textarea>
                              </div>
                            </div>
                          </div>

                          <div class="row mt-5">
                            <div class="text-center">
                              <button
                                v-if="
                                  request_cancel == null ||
                                  request_cancel?.status == 1
                                "
                                type="button"
                                class="btn btn-info btn-sm"
                                @click="salesCancel()"
                              >
                                Request to Cancel Sales
                              </button>
                            </div>
                          </div>
                        </div>

                        <div v-else class="row text-center mt-10">
                          <div class="alert alert-info fw-bold" role="alert">
                            At this time, there is no cancel request for this
                            Sales Plan
                          </div>
                        </div>
                      </div>
                    </form>
                  </div>
                </div>

                <!-- Modal Contact -->
                <div
                  class="modal fade"
                  tabindex="-1"
                  id="addContact"
                  data-bs-backdrop="static"
                >
                  <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content">
                      <div class="modal-header">
                        <h3 v-if="modal_contact" class="modal-title">
                          Add Contact Person
                        </h3>
                        <h3 v-else class="modal-title">Edit Contact Person</h3>

                        <!--begin::Close-->
                        <div
                          class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                          data-bs-dismiss="modal"
                          aria-label="Close"
                        >
                          <span
                            class="svg-icon svg-icon-1"
                            @click="closeModalContact()"
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
                        <form @submit.prevent="submitContact">
                          <input
                            type="hidden"
                            v-if="sales_detail"
                            v-model="sales_detail.id"
                          />
                          <div class="row">
                            <div class="form-group mb-3">
                              <label class="form-label fw-bold">Name</label>
                              <input
                                type="text"
                                class="form-control"
                                v-model="p_contact.name"
                                :class="{
                                  'is-invalid': errors.name,
                                }"
                              />
                              <span
                                v-if="errors.name"
                                class="error invalid-feedback"
                                >{{ errors.name[0] }}</span
                              >
                            </div>
                            <div class="form-group mb-3">
                              <label class="form-label fw-bold">Phone</label>
                              <input
                                type="number"
                                class="form-control"
                                v-model="p_contact.phone"
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
                              <label class="form-label fw-bold">Email</label>
                              <input
                                type="email"
                                class="form-control"
                                v-model="p_contact.email"
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
                              <label class="form-label fw-bold">Address</label>
                              <input
                                type="text"
                                class="form-control"
                                v-model="p_contact.address"
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
                                v-model="p_contact.title"
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
                          </div>
                          <div class="row mt-10">
                            <div class="col d-flex justify-content-end">
                              <button
                                type="button"
                                class="btn btn-light mx-2"
                                data-bs-dismiss="modal"
                                id="close_modal_contact"
                                @click="closeModalContact()"
                              >
                                Close
                              </button>
                              <button type="submit" class="btn btn-primary">
                                Save
                              </button>
                            </div>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Modal Upload -->
                <div
                  class="modal fade"
                  tabindex="-1"
                  id="addFile"
                  data-bs-backdrop="static"
                >
                  <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content">
                      <div class="modal-header">
                        <h3 v-if="modal_upload == 1" class="modal-title">
                          Upload Attachment RFQ or Email Request
                        </h3>
                        <h3 v-if="modal_upload == 2" class="modal-title">
                          Upload Attachment Workscope
                        </h3>
                        <h3 v-if="modal_upload == 3" class="modal-title">
                          Attachment of Financial Assesment Form (optional)
                        </h3>
                        <h3 v-if="modal_upload == 4" class="modal-title">
                          Attachment of Maintenance Proposal for Customer
                        </h3>
                        <h3 v-if="modal_upload == 5" class="modal-title">
                          Attachment of Profitability Analysis Form Signed
                        </h3>
                        <h3 v-if="modal_upload == 6" class="modal-title">
                          Attachment of Customer Approval (SOW Signed / Proposal
                          Approved)
                        </h3>
                        <h3 v-if="modal_upload == 7" class="modal-title">
                          Attachment of WO/PO number customer document
                        </h3>

                        <!--begin::Close-->
                        <div
                          class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                          data-bs-dismiss="modal"
                          aria-label="Close"
                          id="close_modal_file"
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
                        <form>
                          <div class="form-group mb-3">
                            <input
                              type="file"
                              @change="attachFile()"
                              id="files"
                              ref="files"
                              class="form-control"
                              multiple
                              :class="{
                                'is-invalid': errors.files,
                              }"
                            />
                            <div
                              v-for="(file_error, key) in file_errors"
                              :key="key"
                            >
                              <ul class="list-group mt-3">
                                <li
                                  class="list-group-item text-danger border-0"
                                  v-for="(errorItem, innerKey) in file_error"
                                  :key="innerKey"
                                >
                                  {{ errorItem }}
                                </li>
                              </ul>
                            </div>
                          </div>
                          <div class="row mt-10">
                            <div class="col d-flex justify-content-end">
                              <button
                                type="button"
                                class="btn btn-light mx-2"
                                @click="closeModalFile()"
                              >
                                Close
                              </button>
                              <button
                                type="button"
                                @click="submitFile()"
                                class="btn btn-primary"
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

                <!-- Modal slotRequest -->
                <div
                  class="modal fade"
                  tabindex="-1"
                  id="slotRequest"
                  data-bs-backdrop="static"
                >
                  <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content">
                      <div class="modal-header">
                        <h3 class="modal-title">Request Hangar Slot to CBO</h3>
                        <!--begin::Close-->
                        <div
                          class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                          data-bs-dismiss="modal"
                          aria-label="Close"
                        >
                          <span
                            class="svg-icon svg-icon-1"
                            @click="closeRequestSlot()"
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
                        <form v-if="sales_detail">
                          <div class="mb-3">
                            <label>To <span class="text-danger">*</span></label>
                            <select
                              v-model="user_id"
                              class="form-select"
                              :class="{ 'is-invalid': errors.user_id }"
                            >
                              <option :value="null" disabled>
                                Select User
                              </option>
                              <option
                                v-for="(
                                  user_options, user_index
                                ) in user_option"
                                v-if="user_options.role_id === 5"
                                :value="user_options.id"
                                :key="user_index"
                              >
                                {{ user_options.name }} -
                                {{ user_options.email }}
                              </option>
                            </select>
                            <span
                              v-if="errors.user_id"
                              class="error invalid-feedback"
                              >{{ errors.user_id[0] }}</span
                            >
                          </div>
                          <div class="mb-3">
                            <label
                              >Hangar <span class="text-danger">*</span></label
                            >
                            <select
                              v-model="hangar_id"
                              class="form-select"
                              :class="{ 'is-invalid': errors.hangar_id }"
                            >
                              <option :value="null" disabled>
                                Select Hangar
                              </option>
                              <option
                                v-for="(
                                  hangar_options, hangar_index
                                ) in hangar_option"
                                :value="hangar_options.id"
                                :key="hangar_index"
                              >
                                {{ hangar_options.name }}
                              </option>
                            </select>
                            <span
                              v-if="errors.hangar_id"
                              class="error invalid-feedback"
                              >{{ errors.hangar_id[0] }}</span
                            >
                          </div>
                          <div class="row mt-10">
                            <div class="col d-flex justify-content-end">
                              <button
                                type="button"
                                class="btn btn-danger btn-sm mx-2"
                                data-bs-dismiss="modal"
                                id="close_modal_slot"
                                @click="closeRequestSlot()"
                              >
                                Discard
                              </button>
                              <button
                                type="button"
                                @click="slotRequest()"
                                class="btn btn-primary btn-sm"
                              >
                                Send
                              </button>
                            </div>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Modal notifyUpgrade -->
                <div
                  class="modal fade"
                  tabindex="-1"
                  id="notifyUpgrade"
                  data-bs-backdrop="static"
                >
                  <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content">
                      <div class="modal-header">
                        <h3 class="modal-title">Notify Request to Upgrade</h3>
                        <!--begin::Close-->
                        <div
                          class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                          data-bs-dismiss="modal"
                          aria-label="Close"
                        >
                          <span
                            class="svg-icon svg-icon-1"
                            @click="closeNotifyLevel()"
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
                        <form>
                          <div class="mb-3">
                            <label>To <span class="text-danger">*</span></label>
                            <select
                              v-model="user_id"
                              class="form-select"
                              :class="{ 'is-invalid': errors.user_id }"
                            >
                              <option :value="null" disabled>
                                Select User
                              </option>
                              <option
                                v-for="(
                                  user_options, user_index
                                ) in user_option"
                                v-if="
                                  user_options.role_id === 1 ||
                                  user_options.role_id === 4
                                "
                                :value="user_options.id"
                                :key="user_index"
                              >
                                {{ user_options.name }} -
                                {{ user_options.email }}
                              </option>
                            </select>
                            <span
                              v-if="errors.user_id"
                              class="error invalid-feedback"
                              >{{ errors.user_id[0] }}</span
                            >
                            <small class="text-muted"
                              >Send notification to selected employee</small
                            >
                          </div>
                          <div class="row mt-10">
                            <div class="col d-flex justify-content-end">
                              <button
                                type="button"
                                class="btn btn-danger btn-sm mx-2"
                                data-bs-dismiss="modal"
                                id="close_modal_notify"
                                @click="closeNotifyLevel()"
                              >
                                Discard
                              </button>
                              <button
                                type="button"
                                @click="notifyUpgrade()"
                                class="btn btn-primary btn-sm"
                              >
                                Send
                              </button>
                            </div>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Modal Request Closed Sales -->
                <div
                  class="modal fade"
                  tabindex="-1"
                  id="requestClosedSales"
                  data-bs-backdrop="static"
                >
                  <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content">
                      <div class="modal-header">
                        <h3 class="modal-title">
                          Notify Request to Closed Sales
                        </h3>
                        <!--begin::Close-->
                        <div
                          class="btn btn-icon btn-sm btn-active-light-primary ms-2"
                          data-bs-dismiss="modal"
                          aria-label="Close"
                        >
                          <span
                            class="svg-icon svg-icon-1"
                            @click="closeRequestClosedSales()"
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
                        <form>
                          <div class="mb-3">
                            <label class="form-label">Request to</label>
                            <select
                              v-model="user_id"
                              class="form-select"
                              :class="{ 'is-invalid': errors.user_id }"
                            >
                              <option :value="null" disabled>
                                Select User
                              </option>
                              <option
                                v-for="(
                                  user_options, user_index
                                ) in user_option"
                                v-if="
                                  user_options.role_id === 1 ||
                                  user_options.role_id === 4
                                "
                                :value="user_options.id"
                                :key="user_index"
                              >
                                {{ user_options.name }} -
                                {{ user_options.email }}
                              </option>
                            </select>
                            <span
                              v-if="errors.user_id"
                              class="error invalid-feedback"
                              >{{ errors.user_id[0] }}</span
                            >
                            <small class="text-muted"
                              >Send notification to selected employee</small
                            >
                          </div>
                          <div class="row mt-10">
                            <div class="col d-flex justify-content-end">
                              <button
                                type="button"
                                class="btn btn-danger btn-sm mx-2"
                                data-bs-dismiss="modal"
                                id="close_modal_request_closed"
                                @click="closeRequestClosedSales()"
                              >
                                Discard
                              </button>
                              <button
                                type="button"
                                @click="requestClosedSales()"
                                class="btn btn-primary btn-sm"
                              >
                                Send
                              </button>
                            </div>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- End Tab -->
          </div>
        </div>
      </div>
      <!-- End Detail -->
    </div>
    <!-- End Content -->
  </div>
</template>
<script>
import debounce from 'lodash/debounce'
import moment from 'moment'
import Vue from 'vue'
export default {
  layout: 'template',
  name: 'MySalesPlanDetailPage',
  filters: {
    formatDate(value) {
      if (!value) return ''
      return value.slice(0, 10)
    },
    changeDateFormat(value) {
      if (!value) return ''
      const dateParts = value.split('-')
      return `${dateParts[2]}-${dateParts[1]}-${dateParts[0]}`
    },
  },
  data() {
    return {
      moment: moment,
      uid: this.$auth.user.id,
      user: this.$auth.user.name,
      role: this.$auth.user.role.name,
      sales: {
        data: [],
        link: [],
      },
      p_contact: {
        id: null,
        name: null,
        phone: null,
        email: null,
        address: null,
        title: null,
        status: null,
      },
      initial: null,
      search: null,
      so_number: null,
      type: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      files: [],
      ams: null,
      value: [],
      upgrade: null,
      maintenance_id: null,
      product_id: null,
      maintenance_id: null,
      end_date: null,
      current_date: null,
      maintenance_option: [],
      line: [],
      hangar_id: null,
      hangar_option: [],
      status: null,
      contact_person: [],
      user_option: null,
      modal_contact: false,
      modal_upload: null,
      current_page: null,
      sales_detail: null,
      user_id: null,
      contact: null,
      requirement_id: null,
      sales_id: null,
      files: null,
      filesRevision: [],
      filter: null,
      contacts: null,
      level4: null,
      level3: null,
      level2: null,
      level1: null,
      ams_id: null,
      category: null,
      category_option: null,
      cancel_category_id: null,
      cancel_reason: null,
      file_histories: [],
      file_errors: [],
      flight_hour: null,
      reschedule_date: null,
      reschedule_tat: null,
      product_option: null,

      selectedProduct: null,
      productOptions: [],

      selectedMaintenance: null,
      maintenanceOptions: [],

      selectedAMS: null,
      amsOptions: [],

      sales_update_value: null,
      sales_update_acreg: null,

      errors: {
        ams_id: null,
        name: null,
        phone: null,
        email: null,
        address: null,
        title: null,
        hangar_id: null,
        status: null,
        files: null,
        filesRevision: null,
        so_number: null,
        maintenance_id: null,
        product_id: null,
        ac_reg: null,
        tat: null,
        location: null,
        value: null,
        category: null,
        reason: null,
        user_id: null,
        category_id: null,
        start_date: null,
      },
      sales_history: '',
      flight_hour: '',
      selectedLevel: '',
      sequence: '',
      showSecondSelect: false,
      secondOptions: [],
      showModal: false,
      sales_detail_status: null,
      current_date_reschedule: null,
      transactionType: null,
      data_pbth_rate: null,
      data_pbth_flighthour: null,
      prospect_id: null,
      request_upgrade: null,
      request_hangar: null,
      request_cancel: null,
      request_reschedule: null,
      request_closed: null,
      sales_reschedule: null,
      sales_reject: null,
    }
  },
  mounted() {
    KTStepper.getInstance()
    KTFormRepeaterBasic.init()
    // this.step()
  },
  watch: {
    search: debounce(function () {
      this.listDetail()
      this.listContact()
      this.listFile()
      this.listFileHistory()
    }, 500),
    selectedProduct(newVal, oldVal) {
      if (
        newVal !== null &&
        typeof newVal === 'object' &&
        newVal.hasOwnProperty('id')
      ) {
        this.listMaintenance()
      }
      // Jika selectedProduct berubah, reset selectedMaintenance ke nilai awalnya
      this.selectedMaintenance = []
    },
  },
  created() {
    this.listDetail()
    this.listLog()
    this.listContact()
    this.listFile()
    this.listFileHistory()
    this.listAMS()
    this.listProduct()
    this.listHangar()
    this.listMaintenance()
    this.listUser()
    this.listCategory()
  },
  methods: {
    openModal() {
      this.showModal = true
    },
    closeModal() {
      this.showModal = false
    },
    handleLevelChange() {
      if (this.selectedLevel === 4) {
        this.showSecondSelect = true
        this.secondOptions = [
          { value: '2', label: 'Upload Attachment RFQ or Email Request' },
          { value: '3', label: 'Upload Attachment Workscope' },
        ]
      } else if (this.selectedLevel === 3) {
        this.showSecondSelect = true
        this.secondOptions = [
          {
            value: '5',
            label: 'Attachment of Financial Assesment Form (optional)',
          },
          {
            value: '6',
            label: 'Attachment of Maintenance Proposal for Customer',
          },
          {
            value: '7',
            label: 'Attachment of Profitability Analysis Form Signed',
          },
        ]
      } else if (this.selectedLevel === 2) {
        this.showSecondSelect = true
        this.secondOptions = [
          {
            value: '8',
            label:
              'Attachment of Customer Approval (SOW Signed / Proposal Approved)',
          },
        ]
      } else if (this.selectedLevel === 1) {
        this.showSecondSelect = true
        this.secondOptions = [
          { value: '9', label: 'Attachment of WO/PO number Customer Document' },
        ]
      } else {
        this.showSecondSelect = false
        this.secondOptions = []
      }
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.contact.last_page) {
        this.current_page = this.contact.last_page
      }
      let url = new URL(this.contact.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.listContact(new_url)
    }, 500),

    formatPrice(value) {
      let val = (value / 1).toFixed(0).replace(',', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },

    step() {
      var element = document.querySelector('#kt_stepper_example_basic')
      var stepper = new KTStepper(element)
      stepper.on('kt.stepper.next', function (stepper) {
        stepper.goNext()
      })
      stepper.on('kt.stepper.previous', function (stepper) {
        stepper.goPrevious()
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

    loading2() {
      Swal.fire({
        timer: 5000,
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
      })
    },

    listDetail() {
      // this.loading2()
      this.$axios
        .get(`api/sales-show/${this.$route.query.id}`)
        .then((response) => {
          this.sales_detail = response.data.data.salesDetail
          this.transactionType = response.data.data.salesDetail.type
          this.prospect_id = response.data.data.salesDetail.prospect_id
          this.current_date_reschedule = this.sales_detail.startDate
          this.sales_detail_status = this.sales_detail.status
          this.customer_name = response.data.data.salesDetail.customer.name
          this.level4 = response.data.data.level4
          this.level3 = response.data.data.level3
          this.level2 = response.data.data.level2
          this.level1 = response.data.data.level1
          this.request_upgrade = response.data.data.requestUpgrade
          this.request_hangar = response.data.data.requestHangar
          this.request_cancel = response.data.data.requestCancel
          this.request_reschedule = response.data.data.requestReschedule
          this.request_closed = response.data.data.requestClosed
          this.sales_reschedule = response.data.data.salesReschedule
          this.sales_reject = response.data.data.salesReject
          // this.listPBTH()

          this.sales_update_value = response.data.data.salesDetail.totalSales
          this.sales_update_acreg = response.data.data.salesDetail.acReg
          this.listLog()
          Swal.close()
        })
        .catch((error) => {
          if (error.response.status == 404) {
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: error.response.data.message,
            })
            this.$router.push({
              name: 'my-salesplan-table',
            })
          } else if (error.response.status == 403) {
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: error.response.data.message,
            })
            this.$router.push({
              name: 'my-salesplan-table',
            })
          }
        })
    },

    listLog() {
      this.$axios
        .get(`api/sales-detail-history/${this.$route.query.id}`)
        .then((response) => {
          this.sales_history = response.data.logs
          Swal.close()
        })
        .catch((error) => {
          if (error.response.status == 404 || error.response.status == 403) {
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: error.response.data.message,
            })
            this.$router.push({
              name: 'my-salesplan-table',
            })
          }
        })
    },

    // listPBTH() {
    //   this.$axios
    //     .get(`api/prospect-pbth/` + this.prospect_id)
    //     .then((response) => {
    //       this.data_pbth_flighthour = response.data.data.prospect[0].flight_hour
    //       this.data_pbth_rate = response.data.data.prospect[0].rate
    //     })
    // },

    listContact(paginate) {
      this.loading()
      paginate = paginate || `/api/contact-person`
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
          this.contact = response.data.data.data
          this.current_page = this.contact.current_page
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    listFile() {
      this.loading()
      this.$axios
        .get(`api/file`)
        .then((response) => {
          this.files = response.data.data
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    listFileHistory() {
      this.loading()
      this.$axios
        .get(`api/file-history/${this.$route.query.id}`, {
          params: {
            filter: this.filter,
          },
        })
        .then((response) => {
          this.file_histories = response.data.data.history
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    listAMS() {
      this.loading()
      this.$axios
        .get(`api/ams`)
        .then((response) => {
          this.amsOptions = response.data.data.data.map((amss) => ({
            id: amss.id,
            displayText: `${amss.initial} - ${amss.user.name}`,
            isDisabled:
              this.sales_detail.ams === `${amss.initial} - ${amss.user.name}`,
            class: {
              'bg-secondary':
                this.sales_detail.ams === `${amss.initial} - ${amss.user.name}`,
            },
          }))

          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    listProduct() {
      this.loading()
      this.$axios
        .get(`api/product`)
        .then((response) => {
          this.productOptions = response.data.data.data
          // Pilih produk berdasarkan sales_detail.product.name jika ada
          if (this.sales_detail.product) {
            this.selectedProduct = this.productOptions.find(
              (data) => data.name === this.sales_detail.product.name
            )
          }
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    listMaintenance() {
      if (this.selectedProduct) {
        this.$axios
          .get('api/maintenance', {
            params: {
              product_id: this.selectedProduct.id, // Mengirim ID produk yang dipilih sebagai parameter
              order: 'created_at',
              by: 'ASC',
            },
          })
          .then((response) => {
            this.maintenanceOptions = response.data.data.data
            // Initialize selectedMaintenance as an array
            this.selectedMaintenance = []

            // Split the maintenance string into an array based on semicolon (;)
            const maintenanceArray = this.sales_detail.maintenance.split(';')

            // Iterate over maintenanceArray and find corresponding data in maintenanceOptions
            maintenanceArray.forEach((maintenanceName) => {
              const selectedMaintenanceData = this.maintenanceOptions.find(
                (data) => data.name.trim() === maintenanceName.trim()
              )

              // If the maintenance is found in maintenanceOptions, add it to selectedMaintenance
              if (selectedMaintenanceData) {
                this.selectedMaintenance.push(selectedMaintenanceData)
              }
            })
          })
          .catch((error) => console.log(error))
      } else {
        // Handle the case when no product is selected
        // You can clear the maintenance options or show a message to select a product first
        this.maintenanceOptions = []
        this.selectedMaintenance = []
      }
    },

    listHangar() {
      this.$axios
        .get('api/hangar', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.hangar_option = response.data.data
        })
        .catch((error) => console.error(error))
    },

    listUser() {
      this.loading()
      this.$axios
        .get(`api/users`)
        .then((response) => {
          this.user_option = response.data.data.data
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    listCategory() {
      this.loading()
      this.$axios
        .get(`api/cancel-category`)
        .then((response) => {
          this.category_option = response.data.data
          Swal.close()
        })
        .catch((error) => console.log(error))
    },

    switchAMS() {
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, switch AMS!',
      }).then((result) => {
        if (result.isConfirmed) {
          if (this.selectedAMS) {
            this.$axios
              .put(`api/sales-switch-ams/${this.$route.query.id}`, {
                ams_id: this.selectedAMS.id,
              })
              .then((response) => {
                this.listDetail()
                this.listLog()
                this.closeModalAMS()
                setTimeout(() => {
                  Swal.fire({
                    icon: 'success',
                    title: 'Success!',
                    text: response.data.message,
                  })
                }, 2000)
              })
              .catch((error) => {
                setTimeout(() => {
                  this.errors = error.response.data.errors
                  Swal.fire({
                    icon: 'error',
                    title: 'Error!',
                    text: error.response.data.message,
                  })
                }, 2000)
              })
          } else {
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: 'Please select an AMS before switching.',
            })
          }
        }
      })
    },

    inputSoNumber() {
      if (
        this.sales_reschedule &&
        (this.request_reschedule?.status == 2 ||
          this.request_reschedule?.status == 4)
      ) {
        this.showActiveRequestAlert('Reschedule')
      } else if (
        this.request_cancel &&
        (this.request_cancel?.status == 2 || this.request_cancel?.status == 4)
      ) {
        this.showActiveRequestAlert('Cancel')
      } else if (this.level1[1].data == null) {
        Swal.fire({
          title: 'Invalid',
          html: 'Fill out the SO Number first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        Swal.fire({
          title: 'Submit Confirmation',
          html: 'You will submit the SO Number and locked this Sales Plan status to <b>Closed In</b>, continue?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes, submit and close',
          allowOutsideClick: false,
        }).then((result) => {
          if (result.isConfirmed) {
            this.$axios
              .put(`/api/sales-so-number/${this.$route.query.id}`, {
                so_number: this.level1[1].data,
              })
              .then((response) => {
                this.listDetail()
                this.listLog()

                setTimeout(() => {
                  Swal.fire({
                    icon: 'success',
                    title: 'Success!',
                    text: response.data.message,
                  })
                }, 2000)

                // setTimeout(() => {
                //   redirectConfirmation()
                // }, 10000)
              })
              .catch((error) => {
                this.showErrorResponse(error)
              })
          }
        })
      }
    },

    // NOTE: Experimental
    redirectConfirmation() {
      Swal.fire({
        title: 'Redirect Confirmation',
        html: 'This Sales Plan has just closed-in, do you want to see other Sales Plan in table list or keep stay here?',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'See other Sales Plan',
        cancelButtonText: 'Stay here',
        allowOutsideClick: false,
      }).then((result) => {
        if (result.isConfirmed) {
          this.$router.push({
            name: 'my-salesplan',
          })
        }
      })
    },

    salesUpdate() {
      // Pemeriksaan nilai selectedProduct dan selectedMaintenance
      if (
        !this.selectedProduct ||
        !this.selectedMaintenance ||
        !this.sales_update_value
      ) {
        Swal.fire({
          title: 'Invalid',
          html: 'Complete all required fields (<span class="text-danger">*</span>) first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        this.onLoad()
        // Mengambil semua ID maintenance yang dipilih
        const selectedMaintenanceIds = this.selectedMaintenance.map(
          (maintenance) => maintenance.id
        )

        this.$axios
          .put(`/api/sales-update-tmb/${this.$route.query.id}`, {
            // maintenance_id: this.selectedMaintenance.id,
            maintenance_id: selectedMaintenanceIds,
            product_id: this.selectedProduct.id,
            value: this.sales_update_value,
            ac_reg: this.sales_update_acreg,
          })
          .then((response) => {
            Swal.fire({
              icon: 'success',
              title: 'Success!',
              text: response.data.message,
              confirmButtonText: 'Close',
              allowOutsideClick: false,
            }).then((result) => {
              if (result.isConfirmed) {
                this.listDetail()
                this.listLog()
                this.clearFormEditSales()
                this.closeModalEditSales()
              }
            })
          })
          .catch((error) => {
            this.showErrorResponse(error)
          })
      }
    },

    pbthUpdate() {
      this.loading()
      this.$axios
        .put(`/api/sales-update-pbth/` + this.prospect_id, {
          rate: this.data_pbth_rate,
          flight_hour: this.data_pbth_flighthour,
        })
        .then((response) => {
          // this.listPBTH()
          this.listDetail()
          this.listLog()
          this.clearFormEditSales()
          this.closeModalEditSales()
          setTimeout(() => {
            Swal.fire({
              icon: 'success',
              title: 'Success!',
              text: response.data.message,
            })
          }, 2000)
        })
        .catch((error) => {
          setTimeout(() => {
            this.errors = error.response.data.errors
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: error.response.data.message,
            })
          }, 2000)
        })
    },

    slotRequest() {
      if (
        this.sales_reschedule &&
        (this.request_reschedule?.status == 2 ||
          this.request_reschedule?.status == 4)
      ) {
        this.showActiveRequestAlert('Reschedule')
      } else if (
        this.request_cancel &&
        (this.request_cancel?.status == 2 || this.request_cancel?.status == 4)
      ) {
        this.showActiveRequestAlert('Cancel')
      } else if (this.user_id == null || this.hangar_id == null) {
        Swal.fire({
          title: 'Invalid',
          html: 'Complete all required fields first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        this.onLoad()

        this.$axios
          .post(`api/sales-request-hangar`, {
            sales_id: this.$route.query.id,
            hangar_id: this.hangar_id,
            user_id: this.user_id,
            target_url: this.$route.fullPath,
          })
          .then((response) => {
            this.closeRequestSlot()
            this.showSuccessResponse(response)
          })
          .catch((error) => {
            Swal.close()
            this.showErrorResponse(error)
          })
      }
    },
    slotApprove() {
      Swal.fire({
        title: 'Approve Confirmation',
        text: 'You will approve this Hangar Slot request, continue?',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, approve it',
      }).then((result) => {
        if (result.isConfirmed) {
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-hangar/${this.$route.query.id}`, {
              is_approved: 1,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    slotReject() {
      Swal.fire({
        title: 'Reject Confirmation',
        text: 'You will reject this Hangar Slot request, please give the reason bellow',
        input: 'text',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Reject',
        allowOutsideClick: false,
        inputValidator: (value) => {
          if (!value) {
            return 'Fill the reason field first!'
          }
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const reason = result.value
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-hangar/${this.$route.query.id}`, {
              is_approved: 0,
              reject_reason: reason,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    responseHangarRequest() {
      this.onLoad()

      this.$axios
        .put(`api/sales-response-hangar/${this.$route.query.id}`)
        .then((response) => {
          Swal.close()
          this.listDetail()
          this.listLog()
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    requestClosedSales() {
      if (
        this.sales_reschedule &&
        (this.request_reschedule?.status == 2 ||
          this.request_reschedule?.status == 4)
      ) {
        this.showActiveRequestAlert('Reschedule')
      } else if (
        this.request_cancel &&
        (this.request_cancel?.status == 2 || this.request_cancel?.status == 4)
      ) {
        this.showActiveRequestAlert('Cancel')
      } else if (this.user_id == null) {
        Swal.fire({
          title: 'Invalid',
          html: 'Select user you want to be requested first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        this.onLoad()

        this.$axios
          .post(`api/sales-request-closed`, {
            sales_id: this.$route.query.id,
            user_id: this.user_id,
            target_url: this.$route.fullPath,
          })
          .then((response) => {
            this.closeRequestClosedSales()
            this.showSuccessResponse(response)
          })
          .catch((error) => {
            Swal.close()
            this.showErrorResponse(error)
          })
      }
    },
    closedSalesApprove() {
      Swal.fire({
        title: 'Approve Confirmation',
        text: "You will approve this Closed Sales request and it cant't be revert, continue?",
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, approve it',
      }).then((result) => {
        if (result.isConfirmed) {
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-closed/${this.$route.query.id}`, {
              is_approved: true,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    closedSalesReject() {
      Swal.fire({
        title: 'Reject Confirmation',
        text: 'You will reject this Closed Sales request, please give the reason bellow',
        input: 'text',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Reject',
        allowOutsideClick: false,
        inputValidator: (value) => {
          if (!value) {
            return 'Fill the reason field first!'
          }
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const reason = result.value
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-closed/${this.$route.query.id}`, {
              is_approved: false,
              reject_reason: reason,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    responseClosedSalesRequest() {
      this.onLoad()

      this.$axios
        .put(`api/sales-response-closed/${this.$route.query.id}`)
        .then((response) => {
          Swal.close()
          this.listDetail()
          this.listLog()
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    notifyUpgrade() {
      if (
        this.sales_reschedule &&
        (this.request_reschedule?.status == 2 ||
          this.request_reschedule?.status == 4)
      ) {
        this.showActiveRequestAlert('Reschedule')
      } else if (
        this.request_cancel &&
        (this.request_cancel?.status == 2 || this.request_cancel?.status == 4)
      ) {
        this.showActiveRequestAlert('Cancel')
      } else if (this.user_id == null) {
        Swal.fire({
          title: 'Invalid',
          html: 'Select user you want to be requested first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        this.onLoad()

        this.$axios
          .post(`api/sales-request-upgrade`, {
            sales_id: this.$route.query.id,
            user_id: this.user_id,
            target_url: this.$route.fullPath,
          })
          .then((response) => {
            this.closenotifyUpgrade()
            this.showSuccessResponse(response)
          })
          .catch((error) => {
            Swal.close()
            this.showErrorResponse(error)
          })
      }
    },
    upgradeApprove() {
      Swal.fire({
        title: 'Approve Confirmation',
        text: "You will approve this Sales Plan upgrade request and it cant't be revert, continue?",
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, approve it',
      }).then((result) => {
        if (result.isConfirmed) {
          this.onLoad()

          this.$axios
            .put(`api/sales-upgrade-level/${this.$route.query.id}`, {
              is_approved: true,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    upgradeReject() {
      Swal.fire({
        title: 'Reject Confirmation',
        text: 'You will reject this Sales Plan upgrade request, please give the reason bellow',
        input: 'text',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Reject',
        allowOutsideClick: false,
        inputValidator: (value) => {
          if (!value) {
            return 'Fill the reason field first!'
          }
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const reason = result.value
          this.onLoad()

          this.$axios
            .put(`api/sales-upgrade-level/${this.$route.query.id}`, {
              is_approved: false,
              reject_reason: reason,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    responseUpgradeRequest() {
      this.onLoad()

      this.$axios
        .put(`api/sales-response-upgrade/${this.$route.query.id}`)
        .then((response) => {
          Swal.close()
          this.listDetail()
          this.listLog()
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    salesReschedule() {
      if (
        this.request_upgrade &&
        (this.request_upgrade?.status == 2 || this.request_upgrade?.status == 4)
      ) {
        this.showActiveRequestAlert('Upgrade Level')
      } else if (
        this.request_cancel &&
        (this.request_cancel?.status == 2 || this.request_cancel?.status == 4)
      ) {
        this.showActiveRequestAlert('Cancel')
      } else if (
        this.level4[3].data &&
        (this.request_hangar?.status == 2 || this.request_hangar?.status == 4)
      ) {
        this.showActiveRequestAlert('Hangar Slot')
      } else if (
        this.request_closed &&
        (this.request_closed?.status == 2 || this.request_closed?.status == 4)
      ) {
        this.showActiveRequestAlert('Closed Sales')
      } else if (
        this.user_id == null ||
        this.reschedule_date == null ||
        this.reschedule_tat == null
      ) {
        Swal.fire({
          title: 'Invalid',
          html: 'Complete all required fields (<span class="text-danger">*</span>) first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        Swal.fire({
          title: 'Submit Confirmation',
          html: 'You will send <b>reschedule request</b> with email notification to selected user, continue?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Send request',
        }).then((result) => {
          if (result.isConfirmed) {
            this.onLoad()

            this.$axios
              .post(`api/sales-request-reschedule`, {
                sales_id: this.$route.query.id,
                user_id: this.user_id,
                start_date: this.reschedule_date,
                tat: this.reschedule_tat,
                hangar_id: this.hangar_id,
                target_url: this.$route.fullPath,
              })
              .then((response) => {
                this.showSuccessResponse(response)
              })
              .catch((error) => {
                Swal.close()
                this.showErrorResponse(error)
              })
          }
        })
      }
    },
    salesRescheduleApprove() {
      Swal.fire({
        title: 'Approve Confirmation',
        text: "You will approve this Sales Plan reschedule request and it can't be revert, continue?",
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, approve it',
      }).then((result) => {
        if (result.isConfirmed) {
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-reschedule/${this.$route.query.id}`, {
              is_approved: true,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    salesRescheduleReject() {
      Swal.fire({
        title: 'Reject Confirmation',
        text: 'You will reject this Sales Plan reschedule request, please give the reason bellow',
        input: 'text',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Reject',
        allowOutsideClick: false,
        inputValidator: (value) => {
          if (!value) {
            return 'Fill the reason field first!'
          }
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const reason = result.value
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-reschedule/${this.$route.query.id}`, {
              is_approved: false,
              reject_reason: reason,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    responseRescheduleRequest() {
      this.onLoad()

      this.$axios
        .put(`api/sales-response-reschedule/${this.$route.query.id}`)
        .then((response) => {
          Swal.close()
          this.listDetail()
          this.listLog()
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    salesCancel() {
      if (
        this.request_upgrade &&
        (this.request_upgrade?.status == 2 || this.request_upgrade?.status == 4)
      ) {
        this.showActiveRequestAlert('Upgrade Level')
      } else if (
        this.request_reschedule &&
        (this.request_reschedule?.status == 2 ||
          this.request_reschedule?.status == 4)
      ) {
        this.showActiveRequestAlert('Reschedule')
      } else if (
        this.level4[3].data &&
        (this.request_hangar?.status == 2 || this.request_hangar?.status == 4)
      ) {
        this.showActiveRequestAlert('Hangar Slot')
      } else if (
        this.request_closed &&
        (this.request_closed?.status == 2 || this.request_closed?.status == 4)
      ) {
        this.showActiveRequestAlert('Closed Sales')
      } else if (
        this.user_id == null ||
        this.cancel_category_id == null ||
        this.cancel_reason == null
      ) {
        Swal.fire({
          title: 'Invalid',
          html: 'Complete all required fields (<span class="text-danger">*</span>) first',
          icon: 'error',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Ok, got it',
          allowOutsideClick: false,
        })
      } else {
        Swal.fire({
          title: 'Submit Confirmation',
          html: 'You will send <b>cancel request</b> with email notification to selected user, continue?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Send request',
        }).then((result) => {
          if (result.isConfirmed) {
            this.onLoad()

            this.$axios
              .post(`api/sales-request-cancel`, {
                sales_id: this.$route.query.id,
                user_id: this.user_id,
                category_id: this.cancel_category_id,
                reason: this.cancel_reason,
                target_url: this.$route.fullPath,
              })
              .then((response) => {
                this.showSuccessResponse(response)
              })
              .catch((error) => {
                Swal.close()
                this.showErrorResponse(error)
              })
          }
        })
      }
    },
    salesCancelApprove() {
      Swal.fire({
        title: 'Approve Confirmation',
        html: 'You will approve the request and this Sales Plan will be <b>cancelled</b>, continue?',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, approve it',
      }).then((result) => {
        if (result.isConfirmed) {
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-cancel/${this.$route.query.id}`, {
              is_approved: true,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    salesCancelReject() {
      Swal.fire({
        title: 'Reject Confirmation',
        text: 'You will reject this Sales Plan cancel request, please give the reason bellow',
        input: 'text',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Reject',
        allowOutsideClick: false,
        inputValidator: (value) => {
          if (!value) {
            return 'Fill the reason field first!'
          }
        },
      }).then((result) => {
        if (result.isConfirmed) {
          const reason = result.value
          this.onLoad()

          this.$axios
            .put(`api/sales-approve-cancel/${this.$route.query.id}`, {
              is_approved: false,
              reject_reason: reason,
              target_url: this.$route.fullPath,
            })
            .then((response) => {
              this.showSuccessResponse(response)
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    responseCancelRequest() {
      this.onLoad()

      this.$axios
        .put(`api/sales-response-cancel/${this.$route.query.id}`)
        .then((response) => {
          Swal.close()
          this.listDetail()
          this.listLog()
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    showDetailCancel() {
      if (this.sales_reject && this.sales_detail.status == 'Cancel') {
        Swal.fire({
          title: this.sales_reject?.category,
          text: `"${this.sales_reject?.reason}"`,
          icon: 'info',
          confirmButtonColor: '#3085d6',
          confirmButtonText: 'Close',
          allowOutsideClick: false,
        })
      }
    },

    showSuccessResponse(response) {
      Swal.fire({
        icon: 'success',
        title: 'Success!',
        text: response.data.message,
        confirmButtonText: 'Close',
        allowOutsideClick: false,
      }).then((result) => {
        if (result.isConfirmed) {
          this.listDetail()
          this.listLog()
        }
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
    showActiveRequestAlert(request_type) {
      Swal.fire({
        title: 'Unable to Proceed',
        html: `You currently have <b>active ${request_type} request</b> on this Sales Plan, make sure that request has been confirmed or ask the Reviewer to respond it`,
        icon: 'error',
        confirmButtonColor: '#3085d6',
        confirmButtonText: 'Ok, got it',
        allowOutsideClick: false,
      })
    },

    attachFile() {
      this.files = this.$refs.files.files
    },
    submitFile() {
      this.onLoad()

      let formData = new FormData()
      for (var i = 0; i < this.files.length; i++) {
        let file = this.files[i]
        formData.append('files[' + i + ']', file)
      }
      formData.append('sales_id', this.sales_detail.id)
      formData.append('requirement_id', this.sequence)
      this.$axios
        .post('/api/file-create', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then((response) => {
          this.closeModalFile()

          Swal.fire({
            icon: 'success',
            title: 'Success!',
            text: response.data.message,
            confirmButtonText: 'Close',
            allowOutsideClick: false,
          }).then((result) => {
            if (result.isConfirmed) {
              this.listFile()
              this.listDetail()
              this.listFileHistory()
            }
          })
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    attachFileRevision() {
      this.filesRevision = this.$refs.filesRevision.files
    },
    submitFileRevision() {
      this.onLoad()

      let formData = new FormData()
      if (this.filesRevision) {
        for (var i = 0; i < this.filesRevision.length; i++) {
          let file = this.filesRevision[i]
          formData.append('files[]', file)
        }
      }
      formData.append('sales_id', this.sales_detail.id)
      formData.append('requirement_id', this.sequence)
      this.$axios
        .post('/api/file-create', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then((response) => {
          this.closeModalFileRevision()

          Swal.fire({
            icon: 'success',
            title: 'Success!',
            text: response.data.message,
            confirmButtonText: 'Close',
            allowOutsideClick: false,
          }).then((result) => {
            if (result.isConfirmed) {
              this.listFile()
              this.listDetail()
              this.listFileHistory()
            }
          })
        })
        .catch((error) => {
          Swal.close()
          this.showErrorResponse(error)
        })
    },

    removeFile(id) {
      Swal.fire({
        title: 'Delete Confirmation',
        html: "You will delete this file <b>permanently</b> and it can't be reverted, continue?",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
        allowOutsideClick: false,
      }).then((result) => {
        if (result.isConfirmed) {
          this.$axios
            .delete('/api/file-delete/' + id)
            .then((response) => {
              this.closeModalFileRevision()

              Swal.fire({
                icon: 'success',
                title: 'Success!',
                text: response.data.message,
                confirmButtonText: 'Close',
                allowOutsideClick: false,
              }).then((result) => {
                if (result.isConfirmed) {
                  this.listFile()
                  this.listDetail()
                  this.listFileHistory()
                }
              })
            })
            .catch((error) => {
              Swal.close()
              this.showErrorResponse(error)
            })
        }
      })
    },
    // Upload Attachment RFQ or Email Request
    addFile1() {
      this.sequence = 2
      this.modal_upload = 1
    },
    // Upload Attachment Workscope
    addFile2() {
      this.sequence = 3
      this.modal_upload = 2
    },
    // Attachment of Financial Assesment Form (optional)
    addFile3() {
      this.sequence = 5
      this.modal_upload = 3
    },
    // Attachment of Maintenance Proposal for Customer
    addFile4() {
      this.sequence = 6
      this.modal_upload = 4
    },
    // Attachment of Profitability Analysis Form Signed
    addFile5() {
      this.sequence = 7
      this.modal_upload = 5
    },
    // Attachment of Customer Approval (SOW Signed / Proposal Approved)
    addFile6() {
      this.sequence = 8
      this.modal_upload = 6
    },
    // Attachment of WO/PO number Customer Document
    addFile7() {
      this.sequence = 9
      this.modal_upload = 7
    },

    submitContact() {
      if (this.modal_contact) {
        this.createContact()
      } else {
        this.updateContact()
      }
    },
    addContact() {
      this.modal_contact = true
    },
    createContact() {
      this.loading()
      this.$axios
        .post('/api/contact-person-create', {
          id: this.p_contact.id,
          customer_id: this.sales_detail.customer.id,
          sales_id: this.sales_detail.id,
          name: this.p_contact.name,
          phone: this.p_contact.phone,
          email: this.p_contact.email,
          address: this.p_contact.address,
          title: this.p_contact.title,
          status: this.p_contact.status,
        })
        .then((response) => {
          this.closeModalContact()
          setTimeout(() => {
            Swal.fire({
              icon: 'success',
              title: 'Success!',
              text: response.data.message,
            })
          }, 2000)
        })
        .catch((error) => {
          setTimeout(() => {
            this.errors = error.response.data.errors
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: error.response.data.message,
            })
          }, 2000)
        })
    },
    removeContact(id) {
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
              .delete('/api/contact-person-delete/' + id, {
                params: {
                  sales_id: this.sales_detail.id,
                },
              })
              .then((response) => {
                this.listDetail()
                this.listContact()
                setTimeout(() => {
                  Swal.fire({
                    icon: 'success',
                    title: 'Success!',
                    text: response.data.message,
                  })
                }, 2000)
              })
          }
        })
        .catch((error) => {
          setTimeout(() => {
            this.errors = error.response.data.errors
            Swal.fire({
              icon: 'error',
              title: 'Error!',
              text: error.response.data.message,
            })
          }, 2000)
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

    closeModalFileRevision() {
      document.getElementById('close_modal_file_revision').click()
      this.clearFormFile()
    },
    closeModalFile() {
      document.getElementById('close_modal_file').click()
      this.clearFormFile()
    },
    clearFormFile() {
      this.files = null
      this.$refs.files.value = null
      this.errors.files = null
      this.file_errors = null
      this.errors.sales_detail = null
      this.errors.requirement_id = null
      this.selectedLevel = ''
      this.$refs.filesRevision.value = null
      this.errors.filesRevision = null
      this.handleLevelChange()
    },

    closeModalContact() {
      document.getElementById('close_modal_contact').click()
      this.clearFormContact()
      this.listDetail()
      this.listContact()
    },
    clearFormContact() {
      this.p_contact.id = null
      this.p_contact.name = null
      this.p_contact.phone = null
      this.p_contact.email = null
      this.p_contact.address = null
      this.p_contact.title = null
      this.p_contact.status = null
      this.errors.name = null
      this.errors.phone = null
      this.errors.email = null
      this.errors.address = null
      this.errors.title = null
      this.errors.status = null
    },

    closeModalAMS() {
      document.getElementById('close_modal_ams').click()
      this.clearFormAMS()
    },
    clearFormAMS() {
      this.selectedAMS = null
      this.errors.ams_id = null
    },
    closeModalRejectSales() {
      document.getElementById('close_modal_reject_sales').click()
      this.clearFormRejectSales()
    },
    clearFormRejectSales() {
      this.ams_id = null
      this.description = null
      this.errors.rejectSales.ams_id = null
      this.errors.rejectSales.description = null
    },

    clearFormEditSales() {
      this.errors.maintenance_id = null
      this.errors.ac_reg = null
      this.errors.product_id = null
      this.errors.value = null
      this.errors.start_date = null
      this.errors.tat = null
    },
    closeModalEditSales() {
      const closeModalButton = document.getElementById('close_modal_edit_sales')
      if (closeModalButton) {
        closeModalButton.click()
        this.clearFormEditSales()
      }

      // document.getElementById('close_modal_edit_sales').click()
      // this.clearFormEditSales()
    },

    clearFormRequestSlot() {
      this.user_id = null
      this.hangar_id = null
      this.errors.user_id = null
      this.errors.hangar_id = null
    },
    closeRequestSlot() {
      document.getElementById('close_modal_slot').click()
      this.clearFormRequestSlot()
    },

    clearFormNotifyLevel() {
      this.user_id = null
    },
    closeNotifyLevel() {
      document.getElementById('close_modal_notify').click()
      this.clearFormNotifyLevel()
    },

    clearFormnotifyUpgrade() {
      this.user_id = null
      this.hangar_id = null
    },
    closenotifyUpgrade() {
      document.getElementById('close_modal_notify').click()
      this.clearFormnotifyUpgrade()
    },

    clearFormHistory() {
      this.filter = null
    },

    clearFormRequestClosedSales() {
      this.user_id = null
    },
    closeRequestClosedSales() {
      document.getElementById('close_modal_request_closed').click()
      this.clearFormRequestClosedSales()
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

.badge-gmf {
  background-color: #003399;
}

.badge-green {
  background-color: #ade792;
}

#readOnly {
  background-color: #f0f0f5;
}

#heightContact {
  height: 35px;
}

#cardMarketShare {
  border: dotted #04c8c8;
  border-radius: 10px;
  padding-top: 10px;
}

#cardSalesPlan {
  border: dotted #50cd89;
  border-radius: 10px;
  padding-top: 10px;
}

#cardDevisiasi {
  border: dotted #f1416c;
  border-radius: 10px;
  padding-top: 10px;
}

#textMarketShare {
  color: #04c8c8;
}

#textSalesPlan {
  color: #50cd89;
}

#textDevisiasi {
  color: #f1416c;
}

#textSync {
  background-color: #f1e0d0;
  color: #955f2d;
  margin-left: 10px;
}

#textWaiting {
  background-color: #f1e0d0;
  color: #955f2d;
}

#textApproved {
  background-color: #2146c7;
  color: #fff;
}

#bodyAMS {
  background: #f1f5f9;
  padding: 10px;
  border-radius: 8px;
}

#myBorder {
  border: dashed #cde7fe;
}
#myBorder2 {
  border: dashed #ff76ce;
}
</style>
