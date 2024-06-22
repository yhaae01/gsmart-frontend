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
          <li class="breadcrumb-item text-muted">View Prospect TMB</li>
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
            <span
              class="badge text-muted text-bg-light d-block text-start mt-2"
              >{{ transaction_type }}</span
            >
            <h2 class="mt-1">{{ product }}</h2>
            <div class="text-muted fw-semibold fs-5">
              Project for {{ registration ?? '-' }}
            </div>
            <div class="text-muted fs-6">Remark: {{ remarks }}</div>
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
                    <td class="text-start">{{ data?.name ?? '-' }}</td>
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
      <div class="mb-10">
        <div class="card shadow-sm mt-5">
          <div class="card-header">
            <h3 class="card-title fw-bold">Data Prospect</h3>
            <div class="card-toolbar">
              <form>
                <div class="col mx-5">
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
                </div>
              </form>
              <div class="col">
                <button
                  v-if="
                    role == 'AMS' || role == 'AM' || role == 'Administrator'
                  "
                  type="button"
                  class="btn btn-sm btn-primary mx-3"
                  data-bs-toggle="modal"
                  data-bs-target="#modal"
                  @click="add()"
                >
                  Add Sales Plan
                </button>
                <button
                  v-else
                  type="button"
                  class="btn btn-sm btn-primary mx-3"
                  data-bs-toggle="modal"
                  data-bs-target="#modal"
                  @click="add()"
                  disabled
                >
                  Add Sales Plan
                </button>
              </div>
            </div>
          </div>

          <div class="card-body">
            <div class="row d-flex align-items-center"></div>
            <div class="py-5">
              <div class="table-responsive">
                <table class="table table-row-bordered table-row-gray-200 gy-4">
                  <thead>
                    <tr class="fw-bold fs-6 text-gray-800">
                      <th class="text-center">No</th>
                      <!-- NAME -->
                      <th
                        v-if="order == 'customer.name' && by == 'asc'"
                        @click="sort('customer.name', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Customer
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'customer.name' && by == 'desc'"
                        @click="sort('name', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Customer
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('customer.name', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Customer <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The Customer -->

                      <!-- Start ams sorting -->
                      <th
                        v-if="order == 'ams' && by == 'asc' && role != 'AMS'"
                        @click="sort('ams', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        AMS
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="
                          order == 'ams' && by == 'desc' && role != 'AMS'
                        "
                        @click="sort('name', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        AMS
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else-if="role != 'AMS'"
                        @click="sort('ams', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        AMS <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End ams sorting -->

                      <!-- Registration -->
                      <th
                        v-if="order == 'registration' && by == 'asc'"
                        @click="sort('registration', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Registration
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'registration' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Registration
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('registration', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Registration <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The Registration -->

                      <!-- Maintenance -->
                      <th class="text-center">Maintenance</th>
                      <!-- End Of The Maintenance -->

                      <!-- Location -->
                      <th
                        v-if="order == 'location.name' && by == 'asc'"
                        @click="sort('location.name', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Location
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'location.name' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Location
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('location.name', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Location <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The Location -->

                      <!-- Sales Plan -->
                      <th
                        v-if="order == 'salesPlan' && by == 'asc'"
                        @click="sort('salesPlan', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Sales Plan
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'salesPlan' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Sales Plan
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('salesPlan', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Sales Plan <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The Sales Plan -->

                      <!-- TAT -->
                      <th
                        v-if="order == 'tat' && by == 'asc'"
                        @click="sort('tat', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        TAT
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'tat' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        TAT
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('tat', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        TAT <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The TAT -->

                      <!-- Start Date -->
                      <th
                        v-if="order == 'start_date' && by == 'asc'"
                        @click="sort('start_date', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Start Date
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'start_date' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Start Date
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('start_date', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Start Date <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The Start Date -->

                      <!-- End Date -->
                      <th
                        v-if="order == 'end_date' && by == 'asc'"
                        @click="sort('end_date', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        End Date
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'end_date' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        End Date
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('end_date', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        End Date <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The End Date -->

                      <!-- Level -->
                      <th
                        v-if="order == 'level' && by == 'asc'"
                        @click="sort('level', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Level
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'level' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Level
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('level', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Level <i class="fa-solid fa-sort"></i>
                      </th>
                      <!-- End Of The Level -->

                      <!-- Status -->
                      <th
                        v-if="order == 'status' && by == 'asc'"
                        @click="sort('status', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Status
                        <i
                          class="fa-solid fa-sort-up"
                          style="color: black; white-space: nowrap"
                        ></i>
                      </th>

                      <th
                        v-else-if="order == 'status' && by == 'desc'"
                        @click="sort('id', 'desc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Status
                        <i
                          class="fa-solid fa-sort-down"
                          style="color: black"
                        ></i>
                      </th>

                      <th
                        v-else
                        @click="sort('status', 'asc')"
                        class="text-center"
                        style="white-space: nowrap"
                      >
                        Status <i class="fa-solid fa-sort"></i>
                      </th>
                      <th class="text-center">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="(data, tmbSales_index) in tmbSales"
                      :key="tmbSales_index"
                    >
                      <td class="text-center">
                        {{ paginate_tmbSales.from + tmbSales_index }}
                      </td>
                      <td class="text-center">
                        {{ data.customer?.name ?? '-' }}
                      </td>
                      <td class="text-center" v-if="role != 'AMS'">
                        {{ data.ams ?? '-' }}
                      </td>
                      <td class="text-center">
                        <div v-if="data.registration">
                          {{ data.registration }}
                        </div>
                        <div v-else>-</div>
                      </td>
                      <td class="text-center">
                        {{ data.maintenance }}
                      </td>
                      <td class="text-center">
                        <div v-if="data.location">
                          {{ data.location }}
                        </div>
                        <div v-else>-</div>
                      </td>
                      <td class="text-center" style="color: #50cd89">
                        ${{ data.sales_plan }}
                      </td>
                      <td class="text-center">{{ data.tat }}</td>
                      <td class="text-center">{{ data.start_date }}</td>
                      <td class="text-center">{{ data.end_date }}</td>
                      <td class="text-center">{{ data.level }}</td>
                      <td class="text-center">
                        <b>{{ data.status }}</b>
                      </td>
                      <td class="text-center">
                        <nuxt-link
                          v-if="data"
                          :to="{
                            path: '/my-salesplan-detail',
                            query: { id: data.id },
                          }"
                        >
                          <span
                            class="menu-title"
                            data-bs-toggle="tooltip"
                            data-bs-placement="top"
                            title="Redirect to Detail Sales Plan"
                            >Detail</span
                          >
                        </nuxt-link>
                      </td>
                      <!-- <td class="text-center" style="white-space: nowrap">
                        <button
                          class="btn btn-sm btn-light mx-2"
                          data-bs-toggle="modal"
                          data-bs-target="#modal"
                          @click="edit(data, tmbSales)"
                        >
                          <i class="bi bi-pencil-square text-primary"></i>
                        </button>
                        <button
                          class="btn btn-sm btn-light"
                          v-on:click="removeTmbSales(data.id)"
                        >
                          <i class="bi bi-trash-fill text-primary"></i>
                        </button>
                      </td> -->
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
                        @change="listSalesTmb()"
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
                      v-for="(link, link_index) in paginate_tmbSales.links"
                      :key="link_index"
                      class="page-item"
                      :class="{ disabled: !link.url, active: link.active }"
                    >
                      <a
                        href="javascript:void(0)"
                        @click="listSalesTmb(link.url)"
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

      <!-- Modal add contact person -->
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
                  <span v-if="errors.name" class="error invalid-feedback">{{
                    errors.name[0]
                  }}</span>
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
                  <span v-if="errors.email" class="error invalid-feedback">{{
                    errors.email[0]
                  }}</span>
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
                  <span v-if="errors.phone" class="error invalid-feedback">{{
                    errors.phone[0]
                  }}</span>
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
                  <span v-if="errors.address" class="error invalid-feedback">{{
                    errors.address[0]
                  }}</span>
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
                  <span v-if="errors.title" class="error invalid-feedback">{{
                    errors.title[0]
                  }}</span>
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
      <!-- End of Modal add contact person -->

      <!-- Modal add & edit prospect -->
      <div
        class="modal fade"
        tabindex="-1"
        id="modal"
        data-bs-backdrop="static"
      >
        <div class="modal-dialog modal-xl modal-dialog-centered">
          <div class="modal-content py-5">
            <div class="modal-header">
              <h3 class="modal-title" v-if="modal_create">Add Registration</h3>
              <h3 class="modal-title" v-if="modal_update">Edit Registration</h3>
              <div
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
                id="close_modal_sales"
              ></div>
            </div>
            <div class="modal-body">
              <form @submit.prevent="submit">
                <div class="row">
                  <div class="col-md-6">
                    <div class="mb-3">
                      <label for="Maintenance" class="form-label"
                        >Maintenance</label
                      >
                      <multiselect
                        v-model="tmbSale.maintenance_id"
                        :options="maintenance_option"
                        :searchable="true"
                        :multiple="true"
                        :close-on-select="false"
                        :clear-on-select="false"
                        placeholder="Select Maintenance"
                        track-by="id"
                        label="name"
                        :class="{ 'is-invalid': errors.maintenance_id }"
                      ></multiselect>
                      <span
                        v-if="errors.maintenance_id"
                        class="error invalid-feedback"
                        >{{ errors.maintenance_id[0] }}</span
                      >
                    </div>
                    <div class="mb-3">
                      <label for="Registration" class="form-label"
                        >Registration</label
                      >
                      <input
                        type="text"
                        v-model="tmbSale.ac_reg"
                        class="form-control"
                        open-direction="bottom"
                        :class="{ 'is-invalid': errors.ac_reg }"
                      />
                      <span
                        v-if="errors.ac_reg"
                        class="error invalid-feedback"
                        >{{ errors.ac_reg[0] }}</span
                      >
                    </div>
                    <div class="mb-3">
                      <label for="TAT" class="form-label">TAT</label>
                      <input
                        type="number"
                        v-model="tmbSale.tat"
                        class="form-control"
                        :class="{ 'is-invalid': errors.tat }"
                      />
                      <span v-if="errors.tat" class="error invalid-feedback">{{
                        errors.tat[0]
                      }}</span>
                    </div>
                  </div>
                  <div class="col-md-6">
                    <div class="mb-3">
                      <label for="Sales Plan" class="form-label"
                        >Sales Plan</label
                      >
                      <input
                        type="number"
                        v-model="tmbSale.value"
                        class="form-control"
                        :class="{ 'is-invalid': errors.value }"
                      />
                      <span
                        v-if="errors.value"
                        class="error invalid-feedback"
                        >{{ errors.value[0] }}</span
                      >
                    </div>
                    <div class="mb-3">
                      <label for="Start Date" class="form-label"
                        >Start Date</label
                      >
                      <input
                        type="date"
                        v-model="tmbSale.start_date"
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
                  <div class="col-md-12 justify-content-between d-flex">
                    <div class="btn btn-secondary mt-3" data-bs-dismiss="modal">
                      Back
                    </div>
                    <button
                      type="submit"
                      v-if="modal_create"
                      class="btn btn-primary mt-3"
                    >
                      Submit
                    </button>
                    <button type="submit" v-else class="btn btn-primary mt-3">
                      Update
                    </button>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <!-- End of Modal add & edit prospect -->
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
      customer: [],
      maintenance_option: [],
      contact_persons: [],
      tmbSale: {
        ac_reg: null,
        tat: null,
        value: null,
        start_date: null,
        maintenance_id: [],
        maintenance: null,
        location: null,
        startDate: null,
      },
      contact_person: {
        name: null,
        email: null,
        address: null,
        phone: null,
        customer_id: null,
        title: null,
      },
      customer_id: null,
      customer_name: null,
      tmb: [],
      sales_plan: [],
      registration: null,
      remarks: null,
      customer_image: [],
      market_share: [],
      deviation: [],
      tmbSales: [],
      errors: {
        maintenance_id: null,
        ac_reg: null,
        tat: null,
        value: null,
        start_date: null,
        name: null,
        email: null,
        address: null,
        phone: null,
        customer_id: null,
      },
      order: 'id',
      by: 'desc',
      paginate: '10',
      search: null,
      current_page: null,
      modal_create: false,
      modal_update: false,
      initial: null,
      paginate_tmbSales: [],
      value: [],
      customer_options: [],
      transaction_type: null,
      customer_id: null,
      maintenance_value: null,
      product: null,
    }
  },
  created() {
    this.listContactPersons()
    this.listTMB()
    this.listSalesTmb()
    this.listMaintenance()
    this.listCustomer()
    this.checkRole()
    this.listProspect()
  },
  watch: {
    search: debounce(function () {
      this.listSalesTmb()
    }, 500),
  },
  methods: {
    TMBMessage() {
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
    sort(order, by) {
      this.order = order
      this.by = by
      this.listSalesTmb()
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
      toastr.error('Sorry, You Are Not Allowed to Access Prospect TMB Page!')
    },
    checkRole() {
      if (
        this.role == 'AMS' ||
        this.role == 'AM' ||
        this.role == 'Administrator'
      ) {
      } else {
        this.$router.push('/my-prospect')
        this.authMessage()
      }
    },
    formatNumber(value) {
      let val = (value / 1).toFixed(2).replace(',', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
    closeModal() {
      document.getElementById('close_modal_contact_person').click()
      document.getElementById('close_modal_sales').click()
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
    listProspect() {
      this.$axios
        .get(`api/prospect-show/${this.$route.query.id}`)
        .then((response) => {
          this.customer_id = response.data.data.prospect.customer.id
          this.transaction_type =
            response.data.data.prospect.transaction_type?.name ?? '-'
          this.prospect =
            response.data.data.prospect.transaction_type?.name ?? '-'
        })
    },
    listTMB() {
      this.$axios
        .get(`api/prospect-tmb/${this.$route.query.id}`)
        .then((response) => {
          // Data TMB
          this.tmb = response.data.data.prospect
          this.product = response.data.data.prospect[0].product?.name ?? '-'
          // Customer Name
          this.customer_name = response.data.data.customer?.name ?? '-'
          // Customer ID
          this.customer_id = response.data.data.customer.id
          // Customer Image
          this.customer_image = response.data.data.customer.full_path
          // Registration Format
          this.registration = response.data.data.registration
          // Remarks for TMB Prospect
          this.remarks = response.data.data.prospect[0].remarks
          // Sales Plan Value
          this.sales_plan = response.data.data.sales_plan
          // Market Share Value
          this.market_share = response.data.data.market_share
          // Deviation Value
          this.deviation = response.data.data.deviation
          this.maintenance_value = response.data.data.prospect[0].maintenance_id
          this.tmbSale.value = this.market_share
          this.tmbSale.maintenance_id = this.maintenance_value
        })
        .catch((error) => {
          if (error.response.status == 404 || error.response.status == 403) {
            toastr.error(error.response.data.message)
            this.$router.push({
              name: 'my-prospect',
            })
          }
        })
    },
    listMaintenance() {
      this.$axios
        .get('api/maintenance', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.maintenance_option = response.data.data.data
        })
    },
    listSalesTmb(paginate) {
      this.$axios
      this.loading()
      paginate = paginate || `/api/sales-show-tmb/${this.$route.query.id}`
      this.$axios
        .get(paginate, {
          params: {
            search: this.search,
            filter: this.year,
            paginate: this.paginate,
            order: this.order,
            by: this.by,
          },
        })
        .then((response) => {
          this.tmbSales = response.data.sales.data
          // if(this.tmbSales.length > 0) {
          //   this.TMBMessage()
          //   this.$router.push('/my-prospect');
          // }
          this.paginate_tmbSales = response.data.sales
          this.current_page = response.data.sales.current_page
          this.value = response.data
        })
    },
    submit() {
      if (this.modal_create) {
        this.create()
      } else if (this.modal_update) {
        this.update()
      } else {
        this.clearForm()
        this.createContactPerson()
      }
    },
    create() {
      this.loading()
      // const maintenanceId = this.tmbSale.maintenance_id
      //   ? this.tmbSale.maintenance_id.id
      //   : null
      const maintenanceIds = this.tmbSale.maintenance_id
        ? this.tmbSale.maintenance_id.map((m) => m.id)
        : []

      this.$axios
        .post('/api/sales-create-tmb', {
          prospect_id: this.$route.query.id,
          maintenance_id: maintenanceIds,
          // maintenance_id: maintenanceId,
          // maintenance_id: this.tmbSale.maintenance_id,
          ac_reg: this.tmbSale.ac_reg,
          tat: this.tmbSale.tat,
          value: this.tmbSale.value,
          start_date: this.tmbSale.start_date,
          customer_id: this.customer_id,
          is_rkap: 1,
        })
        .then((response) => {
          // this.$router.push('/my-prospect')
          toastr.success(response.data.message)
          this.listSalesTmb()
          this.listTMB()
          this.closeModal()
        })
        .catch((error) => {
          if (error.response.status) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          }
        })
    },
    update() {
      this.loading()
      this.$axios
        .put('/api/sales-update-tmb/' + this.tmbSale.id, {
          prospect_id: this.$route.query.id,
          maintenance_id: this.tmbSale.maintenance_id,
          ac_reg: this.tmbSale.ac_reg,
          tat: this.tmbSale.tat,
          value: this.tmbSale.value,
          start_date: this.tmbSale.start_date,
        })
        .then((response) => {
          toastr.success(response.data.message)
          this.listSalesTmb()
          this.closeModal()
        })
        .catch((error) => {
          if (error.response.status) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          }
        })
    },
    createContactPerson() {
      this.loading()
      this.listProspect()
      this.$axios
        .post('/api/contact-person-create', {
          name: this.contact_person?.name,
          phone: this.contact_person.phone,
          email: this.contact_person.email,
          address: this.contact_person.address,
          customer_id: this.customer_id,
          title: this.contact_person.title,
        })
        .then((response) => {
          this.clearForm()
          toastr.success(response.data.message)
          this.listContactPersons()
          this.closeModal()
        })
        .catch((error) => {
          if (error.response.status) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          }
        })
    },
    edit(tmbSale) {
      this.clearError()
      this.modal_create = false
      this.modal_update = true
      this.tmbSale.id = tmbSale.id
      this.tmbSale.maintenance_id = tmbSale.maintenance.id
      this.tmbSale.ac_reg = tmbSale.registration
      this.tmbSale.tat = tmbSale.tat
      this.tmbSale.value = tmbSale.sales_plan
      this.tmbSale.start_date = tmbSale.start_date
    },
    add() {
      this.modal_create = true
      this.clearForm()
      this.modal_update = false
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
      this.tmbSale.ac_reg = null
      this.tmbSale.tat = null
      this.tmbSale.value = null
      this.tmbSale.start_date = null
      this.tmbSale.maintenance_id = null
      this.contact_person.name = null
      this.contact_person.email = null
      this.contact_person.address = null
      this.contact_person.phone = null
      this.contact_person.title = null
      this.errors.maintenance_id = null
      this.errors.ac_reg = null
      this.errors.tat = null
      this.errors.value = null
      this.errors.start_date = null
      this.errors.name = null
      this.errors.email = null
      this.errors.address = null
      this.errors.phone = null
      this.errors.title = null
    },
    clearError() {
      this.errors.maintenance_id = null
      this.errors.ac_reg = null
      this.errors.tat = null
      this.errors.value = null
      this.errors.start_date = null
    },
    directPage: debounce(function () {
      alert(this.paginate_tmbSales.current_page)
      if (this.current_page < 1) {
        this.paginate_tmbSales.current_page = 1
      } else if (
        this.paginate_tmbSales.current_page > this.paginate_tmbSales.last_page
      ) {
        this.paginate_tmbSales.current_page = this.paginate_tmbSales.last_page
      }
      let url = new URL(this.paginate_tmbSales.first_page_url)
      let filter_params = url.filterParams
      filter_params.set('page', this.paginate_tmbSales.current_page)
      url.filter = filter_params.toString()
      let new_url = url.toString()
      this.listSalesTmb(new_url)
    }, 500),
    removeTmbSales(id) {
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
              .delete('/api/sales-delete-tmb/' + id)
              .then((response) => {
                toastr.success(response.data.message)
                this.listSalesTmb()
              })
          }
        })
        .catch((error) => {
          console.log(error)
        })
    },
    removeContactPerson(id) {
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
  },
}
</script>

<style></style>
