{{
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
            My Prospect
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
          <li class="breadcrumb-item text-muted">My Prospect</li>
          <!--end::Item-->
        </ul>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container-fluid mt-10 mb-20">
      <div class="card shadow-sm mb-5">
        <div class="card-header">
          <h1 class="card-title fw-bold">Your Prospect Statistic</h1>
        </div>
        <div class="card-body">
          <div class="row mb-5">
            <div class="col">
              <div class="row">
                <p class="mb-5 fw-bold text-gray-600">
                  This year earnings calculated. Apart from arranging the order
                  of topics.
                </p>
                <div class="col">
                  <div
                    class="border-dashed p-4"
                    v-if="prospect2.totalMarketShare"
                  >
                    <h1 class="fw-bold mb-0">
                      USD {{ formatNumber(prospect2.totalMarketShare) }}
                    </h1>
                    <p class="mb-0 fw-bold text-gray-500">Total Salesplan</p>
                  </div>
                  <div class="border-dashed p-4" v-else>
                    <h1 class="fw-bold mb-0">USD 0</h1>
                    <p class="mb-0 fw-bold text-gray-500">Total Salesplan</p>
                  </div>
                </div>
                <div class="col">
                  <div
                    class="border-dashed p-4"
                    v-if="prospect2.totalSalesPlan"
                  >
                    <h1 class="fw-bold mb-0">
                      USD {{ formatNumber(prospect2.totalSalesPlan) }}
                    </h1>
                    <p class="mb-0 fw-bold text-gray-500">Total Sales RKAP</p>
                  </div>
                  <div class="border-dashed p-4" v-else>
                    <h1 class="fw-bold mb-0">USD 0</h1>
                    <p class="mb-0 fw-bold text-gray-500">Total Sales RKAP</p>
                  </div>
                </div>
                <div class="col">
                  <div class="border-dashed p-4" v-if="prospect2.deviation">
                    <h1 class="fw-bold mb-0">
                      USD {{ formatNumber(prospect2.deviation) }}
                    </h1>
                    <p class="mb-0 fw-bold text-gray-500">Deviation</p>
                  </div>
                  <div class="border-dashed p-4" v-else>
                    <h1 class="fw-bold mb-0">USD 0</h1>
                    <p class="mb-0 fw-bold text-gray-500">Deviation</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="card-body">
        <div class="card shadow-sm mt-5">
          <div class="card-header">
            <h3 class="card-title fw-bold">My Prospect</h3>
            <div class="card-toolbar">
              <div v-if="role == 'TPC' || role == 'Administrator'">
                <button
                  type="button"
                  class="btn btn-sm btn-primary"
                  data-bs-toggle="modal"
                  data-bs-target="#kt_modal_create_app"
                >
                  Add Prospect
                </button>
              </div>
              <div v-else>
                <button
                  type="button"
                  class="btn btn-sm btn-primary d-none"
                  data-bs-toggle="modal"
                  data-bs-target="#notAuthorized"
                >
                  Add Prospect
                </button>
              </div>

              <!-- Modal -->
              <div
                class="modal fade"
                id="notAuthorized"
                tabindex="-1"
                aria-labelledby="exampleModalLabel"
                aria-hidden="true"
              >
                <div class="modal-dialog">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h1 class="modal-title fs-5" id="exampleModalLabel">
                        Notification
                      </h1>
                      <button
                        type="button"
                        class="btn-close"
                        data-bs-dismiss="modal"
                        aria-label="Close"
                      ></button>
                    </div>
                    <div class="modal-body">
                      Sorry, You Are Not Allowed to Access This Feature
                    </div>
                    <div class="modal-footer">
                      <button
                        type="button"
                        class="btn btn-secondary"
                        data-bs-dismiss="modal"
                      >
                        Close
                      </button>
                    </div>
                  </div>
                </div>
              </div>
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
              <ul
                class="nav nav-tabs nav-line-tabs mb-5 fs-6 justify-content-end mt-5"
              >
                <li class="nav-item">
                  <a
                    class="nav-link active"
                    exact-active-class="active"
                    data-bs-toggle="tab"
                    href="#kt_tab_pane_1"
                    @click="filterYearDefault(year)"
                    >All Year</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    class="nav-link"
                    exact-active-class="active"
                    data-bs-toggle="tab"
                    href="#kt_tab_pane_2"
                    @click="filterYear(currentYear)"
                    >This Year</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    class="nav-link"
                    active-class="active"
                    data-bs-toggle="tab"
                    href="#kt_tab_pane_3"
                    @click="filterYear(currentYear - 1)"
                    >{{ currentYear - 1 }}</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    class="nav-link"
                    active-class="active"
                    data-bs-toggle="tab"
                    href="#kt_tab_pane_4"
                    @click="filterYear(currentYear - 2)"
                    >{{ currentYear - 2 }}</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    class="nav-link"
                    active-class="active"
                    data-bs-toggle="tab"
                    href="#kt_tab_pane_5"
                    @click="filterYear(currentYear - 3)"
                    >{{ currentYear - 3 }}</a
                  >
                </li>
              </ul>
              <div class="tab-content" id="myTabContent">
                <div
                  class="tab-pane fade show active"
                  id="kt_tab_pane_1"
                  role="tabpanel"
                >
                  <div class="table-responsive">
                    <table
                      class="table table-row-bordered table-row-gray-200 gy-4"
                    >
                      <thead>
                        <tr class="fw-bold fs-6 text-gray-800">
                          <th class="text-center">No</th>
                          <th class="text-center" style="white-space: nowrap">
                            Product
                          </th>

                          <!-- Year -->
                          <th
                            v-if="order == 'year' && by == 'asc'"
                            @click="sort('year', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'year' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('year', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Year -->

                          <!-- Transaction -->
                          <th
                            v-if="
                              order == 'transaction_type.name' && by == 'asc'
                            "
                            @click="sort('transaction_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'transaction_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('transaction_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Transaction -->

                          <!-- Type -->
                          <th
                            v-if="order == 'prospect_type.name' && by == 'asc'"
                            @click="sort('prospect_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'prospect_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('prospect_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Type -->

                          <!-- Strategic Init -->
                          <th
                            v-if="
                              order == 'strategic_initiative.name' &&
                              by == 'asc'
                            "
                            @click="sort('strategic_initiative.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'strategic_initiative.name' &&
                              by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('strategic_initiative.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Strategic Init -->

                          <!-- PM -->
                          <th
                            v-if="order == 'pm.name' && by == 'asc'"
                            @click="sort('pm.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'pm.name' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('pm.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The PM -->

                          <!-- Customer Code -->
                          <th
                            v-if="order == 'customer.code' && by == 'asc'"
                            @click="sort('customer.code', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'customer.code' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('customer.code', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Customer Code -->

                          <!-- Customer -->
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
                            @click="sort('id', 'desc')"
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

                          <!-- AMS -->
                          <div v-if="role != 'AMS'">
                            <th
                              v-if="order == 'ams.initial' && by == 'asc'"
                              @click="sort('ams.initial', 'desc')"
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
                              v-else-if="order == 'ams.initial' && by == 'desc'"
                              @click="sort('id', 'desc')"
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
                              v-else
                              @click="sort('ams.initial', 'asc')"
                              class="text-center"
                              style="white-space: nowrap"
                            >
                              AMS <i class="fa-solid fa-sort"></i>
                            </th>
                          </div>
                          <!-- End Of The AMS -->

                          <!-- Market Share -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales RKAP
                          </th>
                          <!-- End Of The Market Share -->

                          <!-- Sales Plan -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales Plan
                          </th>
                          <!-- End Of The Sales Plan -->

                          <th class="text-center">Action</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="(data, prospect_index) in prospect"
                          :key="prospect_index"
                        >
                          <td class="text-center">
                            {{ prospect3.from + prospect_index }}
                          </td>
                          <td class="text-center">
                            {{ data.product ?? '-' }}
                          </td>
                          <td class="text-center">{{ data.year }}</td>
                          <td class="text-center">
                            {{ data.transaction_type?.name ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.prospect_type?.name ?? '-' }}
                          </td>

                          <!-- Conditional Strategic Init -->
                          <td>
                            <div
                              v-if="data.strategic_initiative"
                              class="text-center"
                            >
                              {{ data.strategic_initiative?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional Strategic Init -->

                          <!-- Conditional PM -->
                          <td>
                            <div v-if="data.pm" class="text-center">
                              {{ data.pm?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional PM -->

                          <td class="text-center">
                            {{ data.customer?.code ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.customer?.name ?? '-' }}
                          </td>
                          <td v-if="role != 'AMS'" class="text-center">
                            {{ data.ams }}
                          </td>

                          <!-- Conditional Market Share -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.market_share"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.market_share) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Market Share -->

                          <!-- Conditional Sales Value -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.sales_plan) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Sales Value -->

                          <td class="text-center">
                            <div
                              v-if="
                                data.sales_plan && data.transaction_type_id == 3
                              "
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light disabled"
                              >
                                Done
                              </nuxt-link>
                              <!-- TODO Kalau jadinya tidak done -->
                              <!-- <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light"
                              >
                                Pick Up
                              </nuxt-link> -->
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <div v-if="role == 'AM' || role == 'AMS'">
                                <nuxt-link
                                  v-if="
                                    data.transaction_type_id == 1 ||
                                    data.transaction_type_id == 2
                                  "
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                                <nuxt-link
                                  v-else
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                              <div v-else>
                                <nuxt-link
                                  v-if="
                                    data.transaction_type_id == 1 ||
                                    data.transaction_type_id == 2
                                  "
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Detail
                                </nuxt-link>
                                <nuxt-link
                                  v-else
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Detail
                                </nuxt-link>
                              </div>
                            </div>
                          </td>
                        </tr>
                        <tr v-if="prospect.length < 1">
                          <td colspan="12">
                            <div class="text-muted text-center">
                              Data not found
                            </div>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
                <div class="tab-pane fade" id="kt_tab_pane_2" role="tabpanel">
                  <div class="table-responsive">
                    <table
                      class="table table-row-bordered table-row-gray-200 gy-4"
                    >
                      <thead>
                        <tr class="fw-bold fs-6 text-gray-800">
                          <th class="text-center">No</th>

                          <th class="text-center" style="white-space: nowrap">
                            Product
                          </th>
                          <!-- Year -->
                          <th
                            v-if="order == 'year' && by == 'asc'"
                            @click="sort('year', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'year' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('year', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Year -->

                          <!-- Transaction -->
                          <th
                            v-if="
                              order == 'transaction_type.name' && by == 'asc'
                            "
                            @click="sort('transaction_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'transaction_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('transaction_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Transaction -->

                          <!-- Type -->
                          <th
                            v-if="order == 'prospect_type.name' && by == 'asc'"
                            @click="sort('prospect_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'prospect_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('prospect_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Type -->

                          <!-- Strategic Init -->
                          <th
                            v-if="
                              order == 'strategic_initiative.name' &&
                              by == 'asc'
                            "
                            @click="sort('strategic_initiative.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'strategic_initiative.name' &&
                              by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('strategic_initiative.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Strategic Init -->

                          <!-- PM -->
                          <th
                            v-if="order == 'pm.name' && by == 'asc'"
                            @click="sort('pm.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'pm.name' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('pm.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The PM -->

                          <!-- Customer Code -->
                          <th
                            v-if="order == 'customer.code' && by == 'asc'"
                            @click="sort('customer.code', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'customer.code' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('customer.code', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Customer Code -->

                          <!-- Customer -->
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
                            @click="sort('id', 'desc')"
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
                          <!-- AMS -->
                          <th
                            v-if="order == 'ams.initial' && by == 'asc'"
                            @click="sort('ams.initial', 'desc')"
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
                            v-else-if="order == 'ams.initial' && by == 'desc'"
                            @click="sort('id', 'desc')"
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
                            v-else
                            @click="sort('ams.initial', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            AMS <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The AMS -->

                          <!-- Market Share -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales RKAP
                          </th>
                          <!-- End Of The Market Share -->

                          <!-- Sales Plan -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales Plan
                          </th>
                          <!-- End Of The Sales Plan -->

                          <th class="text-center">Action</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="(data, prospect_index) in prospect"
                          :key="prospect_index"
                        >
                          <td class="text-center">
                            {{ prospect3.from + prospect_index }}
                          </td>
                          <td class="text-center">
                            {{ data.product ?? '-' }}
                          </td>
                          <td class="text-center">{{ data.year }}</td>
                          <td class="text-center">
                            {{ data.transaction_type.name }}
                          </td>
                          <td class="text-center">
                            {{ data.prospect_type.name }}
                          </td>

                          <!-- Conditional Strategic Init -->
                          <td>
                            <div
                              v-if="data.strategic_initiative"
                              class="text-center"
                            >
                              {{ data.strategic_initiative.name }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional Strategic Init -->

                          <!-- Conditional PM -->
                          <td>
                            <div v-if="data.pm" class="text-center">
                              {{ data.pm.name }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional PM -->

                          <td class="text-center">
                            {{ data.customer?.code ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.customer?.name ?? '-' }}
                          </td>
                          <td v-if="role != 'AMS'" class="text-center">
                            {{ data.ams }}
                          </td>

                          <!-- Conditional Market Share -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.market_share"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.market_share) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Market Share -->

                          <!-- Conditional Sales Value -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.sales_plan) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Sales Value -->

                          <td class="text-center">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light disabled"
                              >
                                Done
                              </nuxt-link>
                              <!-- TODO Kalau jadinya tidak done -->
                              <!-- <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light"
                              >
                                Pick Up
                              </nuxt-link> -->
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <div
                                v-if="
                                  data.transaction_type_id == 1 ||
                                  data.transaction_type_id == 2
                                "
                              >
                                <nuxt-link
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                              <div v-else>
                                <nuxt-link
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                            </div>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
                <div class="tab-pane fade" id="kt_tab_pane_3" role="tabpanel">
                  <div class="table-responsive">
                    <table
                      class="table table-row-bordered table-row-gray-200 gy-4"
                    >
                      <thead>
                        <tr class="fw-bold fs-6 text-gray-800">
                          <th class="text-center">No</th>
                          <th class="text-center" style="white-space: nowrap">
                            Product
                          </th>

                          <!-- Year -->
                          <th
                            v-if="order == 'year' && by == 'asc'"
                            @click="sort('year', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'year' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('year', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Year -->

                          <!-- Transaction -->
                          <th
                            v-if="
                              order == 'transaction_type.name' && by == 'asc'
                            "
                            @click="sort('transaction_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'transaction_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('transaction_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Transaction -->

                          <!-- Type -->
                          <th
                            v-if="order == 'prospect_type.name' && by == 'asc'"
                            @click="sort('prospect_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'prospect_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('prospect_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Type -->

                          <!-- Strategic Init -->
                          <th
                            v-if="
                              order == 'strategic_initiative.name' &&
                              by == 'asc'
                            "
                            @click="sort('strategic_initiative.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'strategic_initiative.name' &&
                              by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('strategic_initiative.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Strategic Init -->

                          <!-- PM -->
                          <th
                            v-if="order == 'pm.name' && by == 'asc'"
                            @click="sort('pm.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'pm.name' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('pm.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The PM -->

                          <!-- Customer Code -->
                          <th
                            v-if="order == 'customer.code' && by == 'asc'"
                            @click="sort('customer.code', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'customer.code' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('customer.code', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Customer Code -->

                          <!-- Customer -->
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
                            @click="sort('id', 'desc')"
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
                          <!-- AMS -->
                          <th
                            v-if="order == 'ams.initial' && by == 'asc'"
                            @click="sort('ams.initial', 'desc')"
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
                            v-else-if="order == 'ams.initial' && by == 'desc'"
                            @click="sort('id', 'desc')"
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
                            v-else
                            @click="sort('ams.initial', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            AMS <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The AMS -->

                          <!-- Market Share -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales RKAP
                          </th>
                          <!-- End Of The Market Share -->

                          <!-- Sales Plan -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales Plan
                          </th>
                          <!-- End Of The Sales Plan -->

                          <th class="text-center">Action</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="(data, prospect_index) in prospect"
                          :key="prospect_index"
                        >
                          <td class="text-center">
                            {{ prospect3.from + prospect_index }}
                          </td>
                          <td class="text-center">
                            {{ data.product ?? '-' }}
                          </td>
                          <td class="text-center">{{ data.year }}</td>
                          <td class="text-center">
                            {{ data.transaction_type?.name ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.prospect_type?.name ?? '-' }}
                          </td>

                          <!-- Conditional Strategic Init -->
                          <td>
                            <div
                              v-if="data.strategic_initiative"
                              class="text-center"
                            >
                              {{ data.strategic_initiative?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional Strategic Init -->

                          <!-- Conditional PM -->
                          <td>
                            <div v-if="data.pm" class="text-center">
                              {{ data.pm?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional PM -->

                          <td class="text-center">
                            {{ data.customer?.code ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.customer?.name ?? '-' }}
                          </td>
                          <td v-if="role != 'AMS'" class="text-center">
                            {{ data.ams }}
                          </td>

                          <!-- Conditional Market Share -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.market_share"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.market_share) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Market Share -->

                          <!-- Conditional Sales Value -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.sales_plan) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Sales Value -->

                          <td class="text-center">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light disabled"
                              >
                                Done
                              </nuxt-link>
                              <!-- TODO Kalau jadinya tidak done -->
                              <!-- <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light"
                              >
                                Pick Up
                              </nuxt-link> -->
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <div
                                v-if="
                                  data.transaction_type_id == 1 ||
                                  data.transaction_type_id == 2
                                "
                              >
                                <nuxt-link
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                              <div v-else>
                                <nuxt-link
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                            </div>
                          </td>
                        </tr>
                        <tr v-if="prospect.length < 1">
                          <td colspan="12">
                            <div class="text-muted text-center">
                              Data not found
                            </div>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
                <div class="tab-pane fade" id="kt_tab_pane_4" role="tabpanel">
                  <div class="table-responsive">
                    <table
                      class="table table-row-bordered table-row-gray-200 gy-4"
                    >
                      <thead>
                        <tr class="fw-bold fs-6 text-gray-800">
                          <th class="text-center">No</th>
                          <th class="text-center" style="white-space: nowrap">
                            Product
                          </th>

                          <!-- Year -->
                          <th
                            v-if="order == 'year' && by == 'asc'"
                            @click="sort('year', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'year' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('year', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Year -->

                          <!-- Transaction -->
                          <th
                            v-if="
                              order == 'transaction_type.name' && by == 'asc'
                            "
                            @click="sort('transaction_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'transaction_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('transaction_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Transaction -->

                          <!-- Type -->
                          <th
                            v-if="order == 'prospect_type.name' && by == 'asc'"
                            @click="sort('prospect_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'prospect_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('prospect_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Type -->

                          <!-- Strategic Init -->
                          <th
                            v-if="
                              order == 'strategic_initiative.name' &&
                              by == 'asc'
                            "
                            @click="sort('strategic_initiative.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'strategic_initiative.name' &&
                              by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('strategic_initiative.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Strategic Init -->

                          <!-- PM -->
                          <th
                            v-if="order == 'pm.name' && by == 'asc'"
                            @click="sort('pm.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'pm.name' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('pm.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The PM -->

                          <!-- Customer Code -->
                          <th
                            v-if="order == 'customer.code' && by == 'asc'"
                            @click="sort('customer.code', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'customer.code' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('customer.code', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Customer Code -->

                          <!-- Customer -->
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
                            @click="sort('id', 'desc')"
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
                          <!-- AMS -->
                          <th
                            v-if="order == 'ams.initial' && by == 'asc'"
                            @click="sort('ams.initial', 'desc')"
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
                            v-else-if="order == 'ams.initial' && by == 'desc'"
                            @click="sort('id', 'desc')"
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
                            v-else
                            @click="sort('ams.initial', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            AMS <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The AMS -->

                          <!-- Market Share -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales RKAP
                          </th>
                          <!-- End Of The Market Share -->

                          <!-- Sales Plan -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales Plan
                          </th>
                          <!-- End Of The Sales Plan -->

                          <th class="text-center">Action</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="(data, prospect_index) in prospect"
                          :key="prospect_index"
                        >
                          <td class="text-center">
                            {{ prospect3.from + prospect_index }}
                          </td>
                          <td class="text-center">
                            {{ data.product ?? '-' }}
                          </td>
                          <td class="text-center">{{ data.year }}</td>
                          <td class="text-center">
                            {{ data.transaction_type?.name ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.prospect_type?.name ?? '-' }}
                          </td>

                          <!-- Conditional Strategic Init -->
                          <td>
                            <div
                              v-if="data.strategic_initiative"
                              class="text-center"
                            >
                              {{ data.strategic_initiative?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional Strategic Init -->

                          <!-- Conditional PM -->
                          <td>
                            <div v-if="data.pm" class="text-center">
                              {{ data.pm?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional PM -->

                          <td class="text-center">
                            {{ data.customer?.code ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.customer?.name ?? '-' }}
                          </td>
                          <td v-if="role != 'AMS'" class="text-center">
                            {{ data.ams }}
                          </td>

                          <!-- Conditional Market Share -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.market_share"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.market_share) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Market Share -->

                          <!-- Conditional Sales Value -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.sales_plan) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Sales Value -->

                          <td class="text-center">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light disabled"
                              >
                                Done
                              </nuxt-link>
                              <!-- TODO Kalau jadinya tidak done -->
                              <!-- <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light"
                              >
                                Pick Up
                              </nuxt-link> -->
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <div v-if="role == 'AMS' || role == 'AM'">
                                <nuxt-link
                                  v-if="
                                    data.transaction_type_id == 1 ||
                                    data.transaction_type_id == 2
                                  "
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                                <nuxt-link
                                  v-else
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                              <div v-else>
                                <nuxt-link
                                  v-if="
                                    data.transaction_type_id == 1 ||
                                    data.transaction_type_id == 2
                                  "
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Detail
                                </nuxt-link>
                                <nuxt-link
                                  v-else
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Detail
                                </nuxt-link>
                              </div>
                            </div>
                          </td>
                        </tr>
                        <tr v-if="prospect.length < 1">
                          <td colspan="12">
                            <div class="text-muted text-center">
                              Data not found
                            </div>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
                <div class="tab-pane fade" id="kt_tab_pane_5" role="tabpanel">
                  <div class="table-responsive">
                    <table
                      class="table table-row-bordered table-row-gray-200 gy-4"
                    >
                      <thead>
                        <tr class="fw-bold fs-6 text-gray-800">
                          <th class="text-center">No</th>
                          <th class="text-center" style="white-space: nowrap">
                            Product
                          </th>

                          <!-- Year -->
                          <th
                            v-if="order == 'year' && by == 'asc'"
                            @click="sort('year', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'year' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('year', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Year <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Year -->

                          <!-- Transaction -->
                          <th
                            v-if="
                              order == 'transaction_type.name' && by == 'asc'
                            "
                            @click="sort('transaction_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'transaction_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('transaction_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Transaction <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Transaction -->

                          <!-- Type -->
                          <th
                            v-if="order == 'prospect_type.name' && by == 'asc'"
                            @click="sort('prospect_type.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'prospect_type.name' && by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('prospect_type.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Type <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Type -->

                          <!-- Strategic Init -->
                          <th
                            v-if="
                              order == 'strategic_initiative.name' &&
                              by == 'asc'
                            "
                            @click="sort('strategic_initiative.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="
                              order == 'strategic_initiative.name' &&
                              by == 'desc'
                            "
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('strategic_initiative.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Strategic Init
                            <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Strategic Init -->

                          <!-- PM -->
                          <th
                            v-if="order == 'pm.name' && by == 'asc'"
                            @click="sort('pm.name', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'pm.name' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('pm.name', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            PM <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The PM -->

                          <!-- Customer Code -->
                          <th
                            v-if="order == 'customer.code' && by == 'asc'"
                            @click="sort('customer.code', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-up"
                              style="color: black; white-space: nowrap"
                            ></i>
                          </th>

                          <th
                            v-else-if="order == 'customer.code' && by == 'desc'"
                            @click="sort('id', 'desc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code
                            <i
                              class="fa-solid fa-sort-down"
                              style="color: black"
                            ></i>
                          </th>

                          <th
                            v-else
                            @click="sort('customer.code', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            Customer Code <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The Customer Code -->

                          <!-- Customer -->
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
                            @click="sort('id', 'desc')"
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
                          <!-- AMS -->
                          <th
                            v-if="order == 'ams.initial' && by == 'asc'"
                            @click="sort('ams.initial', 'desc')"
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
                            v-else-if="order == 'ams.initial' && by == 'desc'"
                            @click="sort('id', 'desc')"
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
                            v-else
                            @click="sort('ams.initial', 'asc')"
                            class="text-center"
                            style="white-space: nowrap"
                          >
                            AMS <i class="fa-solid fa-sort"></i>
                          </th>
                          <!-- End Of The AMS -->

                          <!-- Market Share -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales RKAP
                          </th>
                          <!-- End Of The Market Share -->

                          <!-- Sales Plan -->
                          <th class="text-center" style="white-space: nowrap">
                            Sales Plan
                          </th>
                          <!-- End Of The Sales Plan -->

                          <th class="text-center">Action</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="(data, prospect_index) in prospect"
                          :key="prospect_index"
                        >
                          <td class="text-center">
                            {{ prospect3.from + prospect_index }}
                          </td>
                          <td class="text-center">
                            {{ data.product ?? '-' }}
                          </td>
                          <td class="text-center">{{ data.year }}</td>
                          <td class="text-center">
                            {{ data.transaction_type?.name ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.prospect_type?.name ?? '-' }}
                          </td>

                          <!-- Conditional Strategic Init -->
                          <td>
                            <div
                              v-if="data.strategic_initiative"
                              class="text-center"
                            >
                              {{ data.strategic_initiative?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional Strategic Init -->

                          <!-- Conditional PM -->
                          <td>
                            <div v-if="data.pm" class="text-center">
                              {{ data.pm?.name ?? '-' }}
                            </div>
                            <div v-else class="text-center">-</div>
                          </td>
                          <!-- End Of Conditional PM -->

                          <td class="text-center">
                            {{ data.customer?.code ?? '-' }}
                          </td>
                          <td class="text-center">
                            {{ data.customer?.name ?? '-' }}
                          </td>
                          <td v-if="role != 'AMS'" class="text-center">
                            {{ data.ams }}
                          </td>

                          <!-- Conditional Market Share -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.market_share"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.market_share) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Market Share -->

                          <!-- Conditional Sales Value -->
                          <td style="white-space: nowrap">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD {{ formatNumber(data.sales_plan) }}
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              USD 0
                            </div>
                          </td>
                          <!-- End Of Conditional Sales Value -->

                          <td class="text-center">
                            <div
                              v-if="data.sales_plan"
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light disabled"
                              >
                                Done
                              </nuxt-link>
                              <!-- TODO Kalau jadinya tidak done -->
                              <!-- <nuxt-link
                                :to="{
                                  path: 'view-prospect-pbth',
                                  query: { id: data.id },
                                }"
                                class="btn btn-sm btn-light"
                              >
                                Pick Up
                              </nuxt-link> -->
                            </div>
                            <div
                              v-else
                              class="text-center"
                              style="color: #50cd89"
                            >
                              <div v-if="role == 'AM' || role == 'AMS'">
                                <nuxt-link
                                  v-if="
                                    data.transaction_type_id == 1 ||
                                    data.transaction_type_id == 2
                                  "
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                                <nuxt-link
                                  v-else
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Pick Up
                                </nuxt-link>
                              </div>
                              <div v-else>
                                <nuxt-link
                                  v-if="
                                    data.transaction_type_id == 1 ||
                                    data.transaction_type_id == 2
                                  "
                                  :to="{
                                    path: 'view-prospect-tmb',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Detail
                                </nuxt-link>
                                <nuxt-link
                                  v-else
                                  :to="{
                                    path: 'view-prospect-pbth',
                                    query: { id: data.id },
                                  }"
                                  class="btn btn-sm btn-light"
                                  style="white-space: nowrap"
                                >
                                  Detail
                                </nuxt-link>
                              </div>
                            </div>
                          </td>
                        </tr>
                        <tr v-if="prospect.length < 1">
                          <td colspan="12">
                            <div class="text-muted text-center">
                              Data not found
                            </div>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
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
                        @change="listProspect()"
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
                      v-for="(link, link_index) in prospect3.links"
                      :key="link_index"
                      class="page-item"
                      :class="{ disabled: !link.url, active: link.active }"
                    >
                      <a
                        href="javascript:void(0)"
                        @click="listProspect(link.url)"
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
    </div>
    <div
      class="modal fade"
      id="kt_modal_create_app"
      tabindex="-1"
      aria-hidden="true"
    >
      <!--begin::Modal dialog-->
      <div class="modal-dialog modal-fullscreen">
        <!--begin::Modal content-->
        <div class="modal-content">
          <!--begin::Modal body-->
          <div class="modal-body px-0 py-0">
            <!--begin::Stepper-->
            <div
              class="stepper stepper-pills stepper-column d-flex flex-column flex-lg-row h-100"
              id="kt_stepper_example_vertical"
            >
              <div class="row h-100 w-100">
                <div
                  class="col-md-3 d-flex align-items-start bg-dark text-white"
                >
                  <!--begin::Aside-->
                  <div class="px-10">
                    <!--begin::Nav-->
                    <div class="stepper-nav mt-10 py-10">
                      <!--begin::Step 1-->
                      <div
                        class="stepper-item me-5 current"
                        data-kt-stepper-element="nav"
                      >
                        <!--begin::Wrapper-->
                        <div class="stepper-wrapper d-flex align-items-center">
                          <!--begin::Icon-->
                          <div class="stepper-icon w-40px h-40px">
                            <i class="stepper-check fas fa-check"></i>
                            <span class="stepper-number">1</span>
                          </div>
                          <!--end::Icon-->

                          <!--begin::Label-->
                          <div class="stepper-label">
                            <h6
                              class="stepper-title fs-6 current"
                              data-kt-stepper-text="text"
                            >
                              Prospect Type
                            </h6>
                            <div class="stepper-desc fs-9">
                              Select Prospect Type
                            </div>
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
                        class="stepper-item me-5"
                        data-kt-stepper-element="nav"
                      >
                        <!--begin::Wrapper-->
                        <div class="stepper-wrapper d-flex align-items-center">
                          <!--begin::Icon-->
                          <div class="stepper-icon w-40px h-40px">
                            <i class="stepper-check fas fa-check"></i>
                            <span class="stepper-number">2</span>
                          </div>
                          <!--begin::Icon-->

                          <!--begin::Label-->
                          <div class="stepper-label">
                            <h3 class="stepper-title fs-6">
                              Year and Customer
                            </h3>
                            <div class="stepper-desc fs-9">
                              Select Year and Customer
                            </div>
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
                        class="stepper-item me-5"
                        data-kt-stepper-element="nav"
                      >
                        <!--begin::Wrapper-->
                        <div class="stepper-wrapper d-flex align-items-center">
                          <!--begin::Icon-->
                          <div class="stepper-icon w-40px h-40px">
                            <i class="stepper-check fas fa-check"></i>
                            <span class="stepper-number">3</span>
                          </div>
                          <!--begin::Icon-->

                          <!--begin::Label-->
                          <div class="stepper-label">
                            <h3 class="stepper-title fs-6">
                              Transaction & Project Type
                            </h3>
                            <div class="stepper-desc fs-9">
                              Select Transaction & Project Type
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
                        class="stepper-item me-5"
                        data-kt-stepper-element="nav"
                      >
                        <!--begin::Wrapper-->
                        <div class="stepper-wrapper d-flex align-items-center">
                          <!--begin::Icon-->
                          <div class="stepper-icon w-40px h-40px">
                            <i class="stepper-check fas fa-check"></i>
                            <span class="stepper-number">4</span>
                          </div>
                          <!--begin::Icon-->

                          <!--begin::Label-->
                          <div class="stepper-label">
                            <h3 class="stepper-title fs-6">Complete Form</h3>
                            <div class="stepper-desc fs-9">
                              Fill All Required Field
                            </div>
                          </div>
                          <!--end::Label-->
                        </div>
                        <!--end::Wrapper-->
                      </div>
                      <!--end::Step 4-->
                    </div>
                    <!--end::Nav-->
                  </div>
                </div>
                <div class="col-md-9">
                  <form class="w-100 p-3" @submit.prevent="submit">
                    <div class="row mt-5">
                      <!--begin::Content-->
                      <div class="flex-row-fluid">
                        <!--begin::Form-->
                        <div
                          class="form px-5 py-5 mx-auto"
                          novalidate="novalidate"
                        >
                          <!--begin::Group-->
                          <div class="mb-5 d-flex justify-content-center">
                            <!--begin::Step 1-->
                            <div
                              class="flex-column flex-column w-50 p-3 w-50 p-3 current"
                              data-kt-stepper-element="content"
                              v-if="role == 'TPC' || role == 'Administrator'"
                            >
                              <div class="w-100 p-3">
                                <h3 class="mb-1">Select Prospect Type</h3>
                                <p class="fs-7 mb-5 text-muted">
                                  If you need more info, please check out FAQ
                                  Page
                                </p>
                                <div
                                  class="mt-10"
                                  v-for="(
                                    prospectType, prospect_type_index
                                  ) in prospect_type.data"
                                  :key="prospect_type_index"
                                >
                                  <input
                                    type="radio"
                                    class="btn-check"
                                    name="prospectType"
                                    v-model="prospect_prospect_type_id"
                                    :value="prospectType.id"
                                    :id="prospectType.name"
                                  />
                                  <label
                                    class="btn btn-outline btn-outline-dashed btn-active-light-primary p-7 d-flex align-items-center mb-5"
                                    :for="prospectType.name"
                                  >
                                    <span
                                      class="svg-icon svg-icon-3hx"
                                      v-if="prospect_type_index % 2 == 0"
                                    >
                                      <svg
                                        width="100"
                                        height="100"
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        xmlns="http://www.w3.org/2000/svg"
                                      >
                                        <path
                                          d="M6.28548 15.0861C7.34369 13.1814 9.35142 12 11.5304 12H12.4696C14.6486 12 16.6563 13.1814 17.7145 15.0861L19.3493 18.0287C20.0899 19.3618 19.1259 21 17.601 21H6.39903C4.87406 21 3.91012 19.3618 4.65071 18.0287L6.28548 15.0861Z"
                                          fill="currentColor"
                                        />
                                        <rect
                                          opacity="0.3"
                                          x="8"
                                          y="3"
                                          width="8"
                                          height="8"
                                          rx="4"
                                          fill="currentColor"
                                        />
                                      </svg>
                                    </span>
                                    <span v-else class="svg-icon svg-icon-3hx">
                                      <svg
                                        width="100"
                                        height="100"
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        xmlns="http://www.w3.org/2000/svg"
                                      >
                                        <path
                                          d="M3 2H10C10.6 2 11 2.4 11 3V10C11 10.6 10.6 11 10 11H3C2.4 11 2 10.6 2 10V3C2 2.4 2.4 2 3 2Z"
                                          fill="currentColor"
                                        />
                                        <path
                                          opacity="0.3"
                                          d="M14 2H21C21.6 2 22 2.4 22 3V10C22 10.6 21.6 11 21 11H14C13.4 11 13 10.6 13 10V3C13 2.4 13.4 2 14 2Z"
                                          fill="currentColor"
                                        />
                                        <path
                                          opacity="0.3"
                                          d="M3 13H10C10.6 13 11 13.4 11 14V21C11 21.6 10.6 22 10 22H3C2.4 22 2 21.6 2 21V14C2 13.4 2.4 13 3 13Z"
                                          fill="currentColor"
                                        />
                                        <path
                                          opacity="0.3"
                                          d="M14 13H21C21.6 13 22 13.4 22 14V21C22 21.6 21.6 22 21 22H14C13.4 22 13 21.6 13 21V14C13 13.4 13.4 13 14 13Z"
                                          fill="currentColor"
                                        />
                                      </svg>
                                    </span>
                                    <!--end::Svg Icon-->

                                    <span
                                      class="d-block fw-semibold text-start align-items-center"
                                    >
                                      <span
                                        class="text-dark fw-bold d-block fs-6"
                                      >
                                        {{ prospectType?.name ?? '-' }}
                                      </span>
                                      <p
                                        class="text-muted fw-semibold fs-7 mb-0"
                                      >
                                        {{ prospectType.description }}
                                      </p>
                                    </span>
                                  </label>
                                </div>
                              </div>
                              <div class="row mt-10">
                                <div class="col-md-6">
                                  <button
                                    type="button"
                                    data-bs-dismiss="modal"
                                    class="btn btn-light text-primary mt-5"
                                    @click="closeModal()"
                                  >
                                    Close
                                  </button>
                                </div>
                                <div
                                  class="col-md-6 justify-content-end d-flex"
                                >
                                  <div
                                    v-if="
                                      role == 'TPC' || role == 'Administrator'
                                    "
                                  >
                                    <button
                                      class="btn btn-primary mt-5 align-self-end"
                                      type="button"
                                      @click="nextButtonClick()"
                                      :disabled="
                                        prospect_prospect_type_id == null
                                      "
                                    >
                                      Next
                                    </button>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else></div>
                            <!--end::Step 1-->

                            <!--begin::Step 2-->
                            <div
                              class="flex-column w-50 p-3 w-50 p-3"
                              data-kt-stepper-element="content"
                              v-if="role == 'TPC' || role == 'Administrator'"
                            >
                              <div class="w-100">
                                <h3 class="mb-1">Select Year and Customer</h3>
                                <p class="fs-7 mb-5 text-muted">
                                  If you need more info, please check out FAQ
                                  Page
                                </p>
                                <div class="row">
                                  <div v-if="prospect_prospect_type_id == 1">
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Year
                                      </label>
                                      <input
                                        type="number"
                                        class="form-control form-control-sm"
                                        v-model="prospect_year"
                                      />
                                    </div>
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Customer
                                      </label>
                                      <multiselect
                                        v-model="selected_customer"
                                        :options="customer_options"
                                        placeholder=""
                                        label="label"
                                        @select="listAmsCustomer"
                                        :close-on-select="true"
                                        :clear-on-select="false"
                                      ></multiselect>
                                    </div>
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Area & AMS
                                      </label>
                                      <multiselect
                                        v-if="selected_customer == null"
                                        :disabled="!selected_customer"
                                        v-model="selected_amsCustomer"
                                        :options="amsCustomer_options"
                                        placeholder=""
                                        label="label"
                                      ></multiselect>
                                      <multiselect
                                        v-else
                                        v-model="selected_amsCustomer"
                                        :options="amsCustomer_options"
                                        placeholder=""
                                        label="label"
                                      ></multiselect>
                                    </div>
                                    <div class="row mt-10">
                                      <div class="col-md-6">
                                        <div
                                          v-if="
                                            role == 'TPC' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          <button
                                            type="button"
                                            class="btn btn-light text-primary mt-5"
                                            @click="
                                              backButtonClick(), deleteOrganic()
                                            "
                                          >
                                            Back
                                          </button>
                                        </div>
                                      </div>
                                      <div
                                        class="col-md-6 justify-content-end d-flex"
                                      >
                                        <div
                                          v-if="
                                            role == 'TPC' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          <div v-if="prospect_year == null">
                                            <button
                                              class="btn btn-primary mt-5 align-self-end disabled"
                                              type="button"
                                            >
                                              Next
                                            </button>
                                          </div>
                                          <div
                                            v-else-if="
                                              prospect_year < 2000 ||
                                              prospect_year > 9999
                                            "
                                          >
                                            <button
                                              class="btn btn-primary mt-5 align-self-end disabled"
                                              type="button"
                                            >
                                              Next
                                            </button>
                                          </div>
                                          <div v-else>
                                            <button
                                              class="btn btn-primary mt-5 align-self-end"
                                              type="button"
                                              @click="nextButtonClick()"
                                              :disabled="
                                                prospect_year == null ||
                                                selected_customer == null ||
                                                selected_amsCustomer == null
                                              "
                                            >
                                              Next
                                            </button>
                                          </div>
                                        </div>
                                      </div>
                                    </div>
                                  </div>
                                  <div
                                    v-else-if="prospect_prospect_type_id == 2"
                                  >
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Year
                                      </label>
                                      <input
                                        type="number"
                                        class="form-control form-control-sm"
                                        v-model="prospect_year"
                                      />
                                    </div>
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Customer
                                      </label>
                                      <multiselect
                                        v-model="selected_customer"
                                        :options="customer_options"
                                        placeholder=""
                                        label="label"
                                        @select="listAmsCustomer"
                                        :close-on-select="true"
                                        :clear-on-select="false"
                                      ></multiselect>
                                    </div>
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Area & AMS
                                      </label>
                                      <multiselect
                                        v-if="selected_customer == null"
                                        :disabled="!selected_customer"
                                        v-model="selected_amsCustomer"
                                        :options="amsCustomer_options"
                                        placeholder=""
                                        label="label"
                                      ></multiselect>
                                      <multiselect
                                        v-else
                                        v-model="selected_amsCustomer"
                                        :options="amsCustomer_options"
                                        placeholder=""
                                        label="label"
                                      ></multiselect>
                                    </div>
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Strategic Init
                                      </label>
                                      <multiselect
                                        v-model="selected_strategic_initiative"
                                        :options="strategic_initiative_options"
                                        placeholder=""
                                        label="name"
                                      ></multiselect>
                                    </div>
                                    <div class="mb-3">
                                      <label
                                        for="exampleFormControlInput1"
                                        class="form-label"
                                      >
                                        Project Manager
                                      </label>
                                      <multiselect
                                        v-model="selected_pm"
                                        :options="project_manager_options"
                                        placeholder=""
                                        label="name"
                                      ></multiselect>
                                    </div>
                                    <div class="row mt-10">
                                      <div class="col-md-6">
                                        <div
                                          v-if="
                                            role == 'TPC' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          <button
                                            type="button"
                                            class="btn btn-light text-primary mt-5"
                                            @click="
                                              backButtonClick(),
                                                deleteInOrganic()
                                            "
                                          >
                                            Back
                                          </button>
                                        </div>
                                      </div>
                                      <div
                                        class="col-md-6 justify-content-end d-flex"
                                      >
                                        <div
                                          v-if="
                                            role == 'TPC' ||
                                            role == 'Administrator'
                                          "
                                        >
                                          <div v-if="prospect_year == null">
                                            <button
                                              class="btn btn-primary mt-5 align-self-end disabled"
                                              type="button"
                                            >
                                              Next
                                            </button>
                                          </div>
                                          <div
                                            v-else-if="
                                              prospect_year < 2000 ||
                                              prospect_year > 9999
                                            "
                                          >
                                            <button
                                              class="btn btn-primary mt-5 align-self-end disabled"
                                              type="button"
                                            >
                                              Next
                                            </button>
                                          </div>
                                          <div v-else>
                                            <button
                                              class="btn btn-primary mt-5 align-self-end"
                                              type="button"
                                              @click="nextButtonClick()"
                                              :disabled="
                                                prospect_year == null ||
                                                selected_customer == null ||
                                                selected_amsCustomer == null ||
                                                selected_strategic_initiative ==
                                                  null ||
                                                selected_pm == null
                                              "
                                            >
                                              Next
                                            </button>
                                          </div>
                                        </div>
                                      </div>
                                    </div>
                                  </div>
                                  <div v-else>
                                    <h3 class="mt-5">
                                      Select at least 1 prospect type
                                    </h3>
                                    <div class="col-md-6">
                                      <div
                                        v-if="
                                          role == 'TPC' ||
                                          role == 'Administrator'
                                        "
                                      >
                                        <button
                                          type="button"
                                          class="btn btn-light text-primary mt-5"
                                          @click="backButtonClick()"
                                        >
                                          Back
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else></div>
                            <!--end::Step 2-->

                            <!--begin::Step 3-->
                            <div
                              class="flex-column w-50 p-3"
                              data-kt-stepper-element="content"
                              v-if="role == 'TPC' || role == 'Administrator'"
                            >
                              <div class="w-100">
                                <h3 class="mb-1">Select Transaction Type</h3>
                                <p class="fs-7 mb-5 text-muted">
                                  If you need more info, please check out FAQ
                                  Page
                                </p>
                                <!--begin::Option-->
                                <div
                                  class="mt-10"
                                  v-for="(
                                    transactionType, transactionType_index
                                  ) in transaction_type.data"
                                  :key="transactionType_index"
                                >
                                  <input
                                    type="radio"
                                    class="btn-check"
                                    name="transaction_type"
                                    v-model="prospect_transaction_type_id"
                                    :value="transactionType.id"
                                    :id="transactionType.name"
                                  />
                                  <label
                                    class="btn btn-outline btn-outline-dashed btn-active-light-primary p-7 d-flex align-items-center mb-5"
                                    :for="transactionType.name"
                                  >
                                    <span
                                      class="svg-icon svg-icon-3hx"
                                      v-if="transactionType_index % 2 == 0"
                                    >
                                      <svg
                                        width="100"
                                        height="100"
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        xmlns="http://www.w3.org/2000/svg"
                                      >
                                        <path
                                          d="M6.28548 15.0861C7.34369 13.1814 9.35142 12 11.5304 12H12.4696C14.6486 12 16.6563 13.1814 17.7145 15.0861L19.3493 18.0287C20.0899 19.3618 19.1259 21 17.601 21H6.39903C4.87406 21 3.91012 19.3618 4.65071 18.0287L6.28548 15.0861Z"
                                          fill="currentColor"
                                        />
                                        <rect
                                          opacity="0.3"
                                          x="8"
                                          y="3"
                                          width="8"
                                          height="8"
                                          rx="4"
                                          fill="currentColor"
                                        />
                                      </svg>
                                    </span>
                                    <span v-else class="svg-icon svg-icon-3hx">
                                      <svg
                                        width="100"
                                        height="100"
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        xmlns="http://www.w3.org/2000/svg"
                                      >
                                        <path
                                          d="M3 2H10C10.6 2 11 2.4 11 3V10C11 10.6 10.6 11 10 11H3C2.4 11 2 10.6 2 10V3C2 2.4 2.4 2 3 2Z"
                                          fill="currentColor"
                                        />
                                        <path
                                          opacity="0.3"
                                          d="M14 2H21C21.6 2 22 2.4 22 3V10C22 10.6 21.6 11 21 11H14C13.4 11 13 10.6 13 10V3C13 2.4 13.4 2 14 2Z"
                                          fill="currentColor"
                                        />
                                        <path
                                          opacity="0.3"
                                          d="M3 13H10C10.6 13 11 13.4 11 14V21C11 21.6 10.6 22 10 22H3C2.4 22 2 21.6 2 21V14C2 13.4 2.4 13 3 13Z"
                                          fill="currentColor"
                                        />
                                        <path
                                          opacity="0.3"
                                          d="M14 13H21C21.6 13 22 13.4 22 14V21C22 21.6 21.6 22 21 22H14C13.4 22 13 21.6 13 21V14C13 13.4 13.4 13 14 13Z"
                                          fill="currentColor"
                                        />
                                      </svg>
                                    </span>
                                    <!--end::Svg Icon-->

                                    <span
                                      class="d-block fw-semibold text-start align-items-center"
                                    >
                                      <span
                                        class="text-dark fw-bold d-block fs-6"
                                      >
                                        {{ transactionType?.name ?? '-' }}
                                      </span>
                                      <p
                                        class="text-muted fw-semibold fs-7 mb-0"
                                      >
                                        {{ transactionType.description }}
                                      </p>
                                    </span>
                                  </label>
                                  <!--end::Option-->
                                </div>
                                <!--begin::Option-->
                              </div>
                              <div class="row mt-10">
                                <div class="col-md-6">
                                  <div
                                    v-if="
                                      role == 'TPC' || role == 'Administrator'
                                    "
                                  >
                                    <button
                                      type="button"
                                      class="btn btn-light text-primary mt-5"
                                      @click="
                                        backButtonClick(),
                                          deleteTransactionType()
                                      "
                                    >
                                      Back
                                    </button>
                                  </div>
                                </div>
                                <div
                                  class="col-md-6 justify-content-end d-flex"
                                >
                                  <div
                                    v-if="
                                      role == 'TPC' || role == 'Administrator'
                                    "
                                  >
                                    <button
                                      class="btn btn-primary mt-5 align-self-end"
                                      type="button"
                                      @click="nextButtonClick()"
                                      :disabled="
                                        prospect_transaction_type_id == null
                                      "
                                    >
                                      Next
                                    </button>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div v-else></div>
                            <!--end::Step 3-->

                            <!--begin::Step 4-->
                            <div
                              class="flex-column w-100"
                              data-kt-stepper-element="content"
                              v-if="role == 'TPC' || role == 'Administrator'"
                            >
                              <h3 class="mb-1">Form Add Prospect</h3>
                              <p class="fs-7 mb-5 text-muted">
                                If you need more info, please check out FAQ Page
                              </p>
                              <div
                                v-if="
                                  prospect_transaction_type_id == 1 ||
                                  prospect_transaction_type_id == 2
                                "
                              >
                                <div class="row mb-5">
                                  <div class="col-sm-4">
                                    <div class="input-group mb-3">
                                      <multiselect
                                        v-model="product_value"
                                        placeholder="select product"
                                        label="name"
                                        :options="product_options"
                                      ></multiselect>
                                    </div>
                                  </div>
                                  <div class="col">
                                    <div v-if="product_value == null">
                                      <button
                                        @click="addProspectTMB(product_value)"
                                        class="btn btn-primary rounded"
                                        type="button"
                                        :disabled="product_value == null"
                                      >
                                        Add Product
                                      </button>
                                    </div>
                                    <div v-else>
                                      <div
                                        v-if="
                                          product_value.name == 'Engine & APU'
                                        "
                                      >
                                        <button
                                          @click="
                                            addProspectTMBEngine(product_value)
                                          "
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                        >
                                          Add Product
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          product_value.name == 'Component'
                                        "
                                      >
                                        <button
                                          @click="
                                            addProspectTMBComponent(
                                              product_value
                                            )
                                          "
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                        >
                                          Add Product
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="product_value.name == 'IGTE'"
                                      >
                                        <button
                                          @click="
                                            addProspectTMBIGTE(product_value)
                                          "
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                        >
                                          Add Product
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          product_value.name == 'Learning'
                                        "
                                      >
                                        <button
                                          @click="
                                            addProspectTMBLearning(
                                              product_value
                                            )
                                          "
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                        >
                                          Add Product
                                        </button>
                                      </div>
                                      <div v-else>
                                        <button
                                          @click="addProspectTMB(product_value)"
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                        >
                                          Add Product
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                                <!--begin::Repeater-->
                                <!--begin::Form group-->
                                <div
                                  class="mb-20 mt-20"
                                  v-for="(data, data_index) in tmb"
                                  :key="data_index"
                                >
                                  <div
                                    class="row mb-10 d-flex align-items-center"
                                  >
                                    <h1 class="fw-bold mb-0">
                                      {{ data_index + 1 }}.
                                      {{ data?.name ?? '-' }}
                                    </h1>
                                  </div>
                                  <div class="form-group">
                                    <div
                                      class="form-group row mb-5"
                                      v-for="(item, item_index) in tmb[
                                        data_index
                                      ].product"
                                      :key="item_index"
                                    >
                                      <div class="col-md-3 text-center">
                                        <div
                                          v-if="
                                            item.product_name == 'Engine & APU'
                                          "
                                        >
                                          <label class="form-label">
                                            Engine
                                          </label>
                                          <multiselect
                                            v-model="item.engine"
                                            :options="engine_options"
                                            placeholder=""
                                            label="name"
                                          ></multiselect>
                                          <span
                                            v-if="
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.engine`
                                              ] == null
                                            "
                                          >
                                          </span>
                                          <span class="text-danger" v-else>
                                            {{
                                              returnError(
                                                errors[
                                                  `tmb.${data_index}.product.${item_index}.engine`
                                                ]
                                              )
                                            }}
                                          </span>
                                        </div>
                                        <div
                                          v-else-if="
                                            item.product_name == 'Component'
                                          "
                                        >
                                          <label class="form-label">
                                            Component
                                          </label>
                                          <multiselect
                                            v-model="item.component"
                                            :options="component_options"
                                            placeholder=""
                                            label="name"
                                          ></multiselect>
                                          <span
                                            v-if="
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.component`
                                              ] == null
                                            "
                                          >
                                          </span>
                                          <span class="text-danger" v-else>
                                            {{
                                              returnError(
                                                errors[
                                                  `tmb.${data_index}.product.${item_index}.component`
                                                ]
                                              )
                                            }}
                                          </span>
                                        </div>
                                        <div
                                          v-else-if="
                                            item.product_name == 'IGTE'
                                          "
                                        >
                                          <label class="form-label">
                                            IGTE
                                          </label>
                                          <multiselect
                                            v-model="item.igte"
                                            :options="igte_options"
                                            placeholder=""
                                            label="name"
                                          ></multiselect>
                                          <span
                                            v-if="
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.igte`
                                              ] == null
                                            "
                                          >
                                          </span>
                                          <span class="text-danger" v-else>
                                            {{
                                              returnError(
                                                errors[
                                                  `tmb.${data_index}.product.${item_index}.igte`
                                                ]
                                              )
                                            }}
                                          </span>
                                        </div>
                                        <div
                                          v-else-if="
                                            item.product_name == 'Learning'
                                          "
                                        >
                                          <label class="form-label">
                                            Learning
                                          </label>
                                          <multiselect
                                            v-model="item.learning"
                                            :options="learning_options"
                                            placeholder=""
                                            label="name"
                                          ></multiselect>
                                          <span
                                            v-if="
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.learning`
                                              ] == null
                                            "
                                          >
                                          </span>
                                          <span class="text-danger" v-else>
                                            {{
                                              returnError(
                                                errors[
                                                  `tmb.${data_index}.product.${item_index}.learning`
                                                ]
                                              )
                                            }}
                                          </span>
                                        </div>
                                        <div v-else>
                                          <label class="form-label">
                                            Aircraft Type
                                          </label>
                                          <multiselect
                                            v-model="item.aircraft_type"
                                            :options="acType_options"
                                            placeholder=""
                                            label="name"
                                          ></multiselect>
                                          <span
                                            v-if="
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.aircraft_type`
                                              ] == null
                                            "
                                          >
                                          </span>
                                          <span class="text-danger" v-else>
                                            {{
                                              returnError(
                                                errors[
                                                  `tmb.${data_index}.product.${item_index}.aircraft_type`
                                                ]
                                              )
                                            }}
                                          </span>
                                        </div>
                                      </div>
                                      <div class="col-md-3 text-center">
                                        <label class="form-label">
                                          Sales RKAP
                                        </label>
                                        <input
                                          type="number"
                                          v-model="item.market_share"
                                          class="form-control mb-2 mb-md-0"
                                          :class="{
                                            'is-invalid':
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.market_share`
                                              ],
                                          }"
                                        />

                                        <span
                                          v-if="
                                            errors[
                                              `tmb.${data_index}.product.${item_index}.market_share`
                                            ] == null
                                          "
                                        >
                                        </span>
                                        <span class="text-danger" v-else>
                                          {{
                                            returnError(
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.market_share`
                                              ]
                                            )
                                          }}
                                        </span>
                                      </div>
                                      <div class="col-md-2 text-center">
                                        <label class="form-label">Remark</label>
                                        <input
                                          type="text"
                                          v-model="item.remark"
                                          class="form-control mb-2 mb-md-0"
                                          :class="{
                                            'is-invalid':
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.remark`
                                              ],
                                          }"
                                        />
                                        <span
                                          v-if="
                                            errors[
                                              `tmb.${data_index}.product.${item_index}.remark`
                                            ] == null
                                          "
                                        >
                                        </span>
                                        <span class="text-danger" v-else>
                                          {{
                                            returnError(
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.remark`
                                              ]
                                            )
                                          }}
                                        </span>
                                      </div>
                                      <div class="col-md-3 text-center">
                                        <label class="form-label">
                                          Maintenance Event
                                        </label>
                                        <multiselect
                                          v-model="item.maintenance_id"
                                          :options="maintenance_options"
                                          placeholder=""
                                          label="name"
                                        ></multiselect>
                                        <span
                                          v-if="
                                            errors[
                                              `tmb.${data_index}.product.${item_index}.maintenance_id`
                                            ] == null
                                          "
                                        >
                                        </span>
                                        <span class="text-danger" v-else>
                                          {{
                                            returnError(
                                              errors[
                                                `tmb.${data_index}.product.${item_index}.maintenance_id`
                                              ]
                                            )
                                          }}
                                        </span>
                                      </div>
                                      <div class="col-md-1">
                                        <button
                                          type="button"
                                          @click="
                                            removeTMB(data_index, item_index)
                                          "
                                          class="btn btn-danger mt-3 mt-md-8"
                                        >
                                          Delete
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                  <!--begin::Form group-->
                                  <!-- <div class="form-group mt-5">
                                    <button
                                      data-repeater-create
                                      id="addTMBButton"
                                      class="btn btn-primary btn-sm"
                                      type="button"
                                      @click="addTMB(data_index, data)"
                                    >
                                      <i class="la la-plus"></i>Add
                                    </button>
                                  </div> -->
                                  <!--end::Form group-->
                                </div>
                                <!--end::Form group-->
                                <div class="row mt-10">
                                  <div class="col-md-6">
                                    <button
                                      type="button"
                                      class="btn btn-light text-primary mt-5"
                                      @click="
                                        backButtonClick(),
                                          deleteTransactionDetail()
                                      "
                                    >
                                      Back
                                    </button>
                                  </div>
                                  <div
                                    class="col-md-6 justify-content-end d-flex"
                                  >
                                    <div
                                      v-if="
                                        role == 'TPC' || role == 'Administrator'
                                      "
                                    >
                                      <div
                                        v-if="
                                          prospect_transaction_type_id == 1 &&
                                          prospect_prospect_type_id == 1
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="createTMBOrganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          prospect_transaction_type_id == 1 &&
                                          prospect_prospect_type_id == 2
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="createTMBInorganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          prospect_transaction_type_id == 2 &&
                                          prospect_prospect_type_id == 1
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="createTMBOrganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          prospect_transaction_type_id == 2 &&
                                          prospect_prospect_type_id == 2
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="createTMBInorganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          prospect_transaction_type_id == 3 &&
                                          prospect_prospect_type_id == 1
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="createPBTHOrganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          prospect_transaction_type_id == 3 &&
                                          prospect_prospect_type_id == 2
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="createPBTHInorganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div v-else>
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="product_value == null"
                                          @click="failMessage()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                                <!--end::Repeater-->
                              </div>
                              <div
                                v-else-if="prospect_transaction_type_id == 3"
                              >
                                <div class="row mb-5">
                                  <div class="col">
                                    <div class="input-group mb-3">
                                      <label class="form-label fw-bold">
                                        Select Product
                                      </label>
                                      <multiselect
                                        v-model="product_value"
                                        :options="product_options"
                                        label="name"
                                        placeholder="search and select product"
                                      ></multiselect>
                                    </div>
                                  </div>
                                  <div class="col">
                                    <div class="input-group mb-3">
                                      <label class="form-label fw-bold">
                                        Select Aircraft Type
                                      </label>
                                      <multiselect
                                        v-model="acType_value"
                                        :options="acType_options"
                                        placeholder="select aircraft type"
                                        label="name"
                                      ></multiselect>
                                    </div>
                                  </div>
                                  <div class="col d-flex align-items-end">
                                    <button
                                      class="mb-3 btn btn-primary rounded"
                                      type="button"
                                      :disabled="
                                        product_value == null ||
                                        acType_value == null
                                      "
                                      @click="
                                        addPBTH(product_value, acType_value)
                                      "
                                    >
                                      Add Data
                                    </button>
                                  </div>
                                </div>
                                <div
                                  v-for="(data_pbth, index_pbth) in pbth"
                                  :key="index_pbth"
                                >
                                  <div class="row mt-10 mb-5">
                                    <h2 class="fw-bold mb-4">
                                      {{ index_pbth + 1 }}.
                                      {{
                                        data_pbth.product_name +
                                        ' - ' +
                                        data_pbth.aircraft_type_name
                                      }}
                                    </h2>
                                    <div class="ms-7">
                                      <p class="text-muted mb-1 fw-bold">
                                        Sales RKAP
                                      </p>
                                      <h2 class="fw-bold">
                                        ${{ data_pbth.market_share }}
                                      </h2>
                                      <p
                                        class="text-muted mb-1 mt-8 fw-bold fs-6"
                                      >
                                        Target
                                      </p>
                                      <div
                                        class="row d-flex justify-content-between"
                                      >
                                        <div
                                          class="col-sm-6"
                                          v-for="(
                                            target_month, target_index
                                          ) in data_pbth.target"
                                          :key="target_index"
                                        >
                                          <div class="row">
                                            <div
                                              class="col-md-2 d-flex align-items-center"
                                            >
                                              <h3 class="fs-7">
                                                {{ target_month.month }}
                                              </h3>
                                            </div>
                                            <div class="col-md-5">
                                              <div class="mb-3">
                                                <label
                                                  class="form-label text-muted fs-7"
                                                >
                                                  Target Rate
                                                </label>
                                                <input
                                                  required
                                                  @change="countMarketShare()"
                                                  type="number"
                                                  v-model="target_month.rate"
                                                  class="form-control form-control-sm"
                                                />
                                                <span
                                                  v-if="
                                                    errors[
                                                      `pbth.${index_pbth}.target.${target_index}.rate`
                                                    ] == null
                                                  "
                                                >
                                                </span>
                                                <span
                                                  class="text-danger"
                                                  v-else
                                                >
                                                  {{
                                                    returnError(
                                                      errors[
                                                        `pbth.${index_pbth}.target.${target_index}.rate`
                                                      ]
                                                    )
                                                  }}
                                                </span>
                                              </div>
                                            </div>
                                            <div class="col-md-5">
                                              <div class="mb-3">
                                                <label
                                                  class="form-label text-muted fs-7"
                                                >
                                                  Target FH
                                                </label>
                                                <input
                                                  required
                                                  @change="countMarketShare()"
                                                  type="number"
                                                  v-model="
                                                    target_month.flight_hour
                                                  "
                                                  class="form-control form-control-sm"
                                                />
                                                <span
                                                  v-if="
                                                    errors[
                                                      `pbth.${index_pbth}.target.${target_index}.flight_hour`
                                                    ] == null
                                                  "
                                                >
                                                </span>
                                                <span
                                                  class="text-danger"
                                                  v-else
                                                >
                                                  {{
                                                    returnError(
                                                      errors[
                                                        `pbth.${index_pbth}.target.${target_index}.flight_hour`
                                                      ]
                                                    )
                                                  }}
                                                </span>
                                              </div>
                                            </div>
                                          </div>
                                        </div>
                                      </div>
                                    </div>
                                    <div class="row mt-5">
                                      <div class="col">
                                        <button
                                          type="button"
                                          class="btn btn-danger btn-sm ms-5"
                                          @click="removePBTH(index_pbth)"
                                        >
                                          <i class="la la-trash-o"></i> Delete
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                                <div class="row mt-10">
                                  <div class="col-md-6">
                                    <button
                                      type="button"
                                      class="btn btn-light text-primary mt-5"
                                      @click="
                                        backButtonClick(),
                                          deleteTransactionDetail()
                                      "
                                    >
                                      Back
                                    </button>
                                  </div>
                                  <div
                                    class="col-md-6 justify-content-end d-flex"
                                  >
                                    <div
                                      v-if="
                                        role == 'TPC' || role == 'Administrator'
                                      "
                                    >
                                      <div
                                        v-if="
                                          prospect_transaction_type_id == 3 &&
                                          prospect_prospect_type_id == 1
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="
                                            product_value == null ||
                                            acType_value == null
                                          "
                                          @click="createPBTHOrganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div
                                        v-else-if="
                                          prospect_transaction_type_id == 3 &&
                                          prospect_prospect_type_id == 2
                                        "
                                      >
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="
                                            product_value == null ||
                                            acType_value == null
                                          "
                                          @click="createPBTHInorganic()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                      <div v-else>
                                        <button
                                          class="btn btn-primary rounded"
                                          type="button"
                                          :disabled="
                                            product_value == null ||
                                            acType_value == null
                                          "
                                          @click="failMessage()"
                                        >
                                          Save
                                        </button>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                              </div>
                              <div v-else>
                                <h3 class="mt-5">
                                  Select at least 1 Transaction type
                                </h3>
                              </div>
                            </div>
                            <div v-else></div>
                            <!--end::Step 4-->
                          </div>
                          <!--end::Group-->
                        </div>
                        <!--end::Form-->
                      </div>
                    </div>
                    <div
                      class="row d-flex justify-content-center me-20 ms-20 d-none"
                    >
                      <div class="col-md-6">
                        <button
                          type="button"
                          data-bs-dismiss="modal"
                          class="btn btn-light text-primary mt-5"
                          id="close_modal"
                        >
                          Close
                        </button>
                        <div v-if="role == 'TPC' || role == 'Administrator'">
                          <button
                            type="button"
                            class="btn btn-light text-primary mt-5"
                            data-kt-stepper-action="previous"
                            @click="backStep()"
                            id="previous_step"
                          >
                            Back
                          </button>
                        </div>
                      </div>
                      <div class="col-md-6 d-flex justify-content-end">
                        <!--begin::Actions-->
                        <div v-if="role == 'TPC' || role == 'Administrator'">
                          <button
                            class="btn btn-primary mt-5 align-self-end"
                            type="button"
                            data-kt-stepper-action="next"
                            @click="nextStep()"
                            id="next_step"
                          >
                            Next
                          </button>
                        </div>
                        <!--begin::Wrapper-->
                        <span
                          class="indicator-label btn d-none"
                          id="close_modal"
                          data-bs-dismiss="modal"
                          >Close</span
                        >
                        <!--end::Wrapper-->

                        <!--begin::Wrapper-->
                        <button
                          type="button"
                          class="btn btn-primary mt-5"
                          data-kt-stepper-action="submit"
                        >
                          <div
                            v-if="
                              prospect_transaction_type_id == 1 &&
                              prospect_prospect_type_id == 1
                            "
                          >
                            <span
                              class="indicator-label"
                              @click="createTMBOrganic()"
                              >Save</span
                            >
                          </div>
                          <div
                            v-else-if="
                              prospect_transaction_type_id == 2 &&
                              prospect_prospect_type_id == 2
                            "
                          >
                            <span
                              class="indicator-label"
                              @click="createPBTHInorganic()"
                              >Save</span
                            >
                          </div>
                          <div
                            v-else-if="
                              prospect_transaction_type_id == 1 &&
                              prospect_prospect_type_id == 2
                            "
                          >
                            <span
                              class="indicator-label"
                              @click="createTMBInorganic()"
                              >Save</span
                            >
                          </div>
                          <div
                            v-else-if="
                              prospect_transaction_type_id == 2 &&
                              prospect_prospect_type_id == 1
                            "
                          >
                            <span
                              class="indicator-label"
                              @click="createPBTHOrganic()"
                              >Save</span
                            >
                          </div>
                          <div v-else>
                            <span class="indicator-label" @click="failMessage()"
                              >Save</span
                            >
                          </div>
                          <span class="indicator-progress">
                            Please wait...
                            <span
                              class="spinner-border spinner-border-smalign-middlems-2"
                            ></span>
                          </span>
                        </button>
                        <!--end::Wrapper-->
                        <!--end::Actions-->
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </div>
            <!--end::Stepper-->
          </div>
          <!--end::Modal body-->
        </div>
        <!--end::Modal content-->
      </div>
      <!--end::Modal dialog-->
    </div>
  </div>
</template>

<script>
import debounce from 'lodash/debounce'
export default {
  layout: 'template',
  name: 'MyProspectPage',
  layout: 'template',
  data() {
    return {
      role: this.$auth.user.role.name,
      prospect_transaction_type_id: null,
      prospect_prospect_type_id: null,
      acType_value: null,
      currentYear: new Date().getFullYear(),
      selected_customer: null,
      selected_amsCustomer: null,
      selected_strategic_initiative: null,
      selected_pm: null,
      product_value: null,
      customer_value: null,
      strategic_initiative_value: null,
      project_manager_value: null,
      areaAms_value: null,
      amsCustomer_options: [],
      acType_options: [],
      engine_options: [],
      igte_options: [],
      learning_options: [],
      component_options: [],
      maintenance_options: [],
      product_options: [],
      aircraft_type_options: [],
      customer_options: [],
      strategic_initiative_options: [],
      project_manager_options: [],
      prospect: [],
      prospect2: [],
      prospect3: [],
      step_number: 1,
      prospect_type: {
        data: [],
      },
      transaction_type: {
        data: [],
      },
      transaction: {
        id: null,
        name: null,
        description: null,
      },
      tmb: [],
      product: [],
      product_id: null,
      pbth: [],
      modal_create: false,
      search: null,
      order: 'id',
      by: 'desc',
      paginate: '10',
      filter: null,
      current_page: null,
      errors: [],
      year: null,
      market_share: null,
      prospect_year: null,
      apu_options: null,
    }
  },
  created() {
    this.listProduct()
    this.listAircraftType()
    this.listProspectType()
    this.listTransactionType()
    this.listCustomer()
    this.listStrategicInitiative()
    this.listUser()
    this.listProspect()
    this.listAcType()
    this.listMaintenance()
    this.checkRole()
    this.listEngine()
    this.listComponent()
    this.listAPU()
    this.listIGTE()
    this.listLearning()
  },
  mounted() {
    KTStepper.getInstance()
    this.step()
  },
  watch: {
    filter: debounce(function () {
      this.listProspect()
    }, 500),
    search: debounce(function () {
      this.listProspect()
    }, 500),
  },
  methods: {
    returnError(errorMessage) {
      let val = errorMessage.toString()
      return val.slice(0, -1)
    },
    formatNumber(value) {
      let val = (value / 1).toFixed(2).replace(',', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
    deleteOrganic() {
      this.prospect_year = null
      this.selected_customer = null
      this.selected_amsCustomer = null
    },
    deleteInOrganic() {
      this.prospect_year = null
      this.selected_customer = null
      this.selected_amsCustomer = null
      this.selected_strategic_initiative = null
      this.selected_pm = null
    },
    deleteTransactionType() {
      this.prospect_transaction_type_id = null
    },
    deleteTransactionDetail() {
      this.product_value = null
      this.acType_value = null
      this.tmb = []
      this.pbth = []
    },
    sort(order, by) {
      this.order = order
      this.by = by
      this.listProspect()
    },
    failMessage() {
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
      toastr.error('Please completed your form!')
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
      toastr.error('Sorry, You Are Not Allowed to Access Prospect Page!')
    },
    checkRole() {
      if (this.role == 'Initial' || this.role == 'CBO') {
        this.$router.push('/')
        this.authMessage()
      } else {
      }
    },
    countMarketShare() {
      this.pbth.forEach((element) => {
        element.market_share = 0
        element.target.forEach((target) => {
          element.market_share += target.rate * target.flight_hour
        })
      })
    },
    nextStep() {
      this.step_number++
    },
    backStep() {
      this.step_number--
    },
    addProspectTMB(value) {
      this.tmb.push({
        id: value.id,
        name: value.name,
        product: [
          {
            product_id: value.id,
            product_name: value.name,
            aircraft_type: null,
            market_share: null,
            remark: null,
            maintenance_id: null,
          },
        ],
      })
    },
    addProspectTMBEngine(value) {
      this.tmb.push({
        id: value.id,
        name: value.name,
        product: [
          {
            product_id: value.id,
            product_name: value.name,
            engine: null,
            market_share: null,
            remark: null,
            maintenance_id: null,
          },
        ],
      })
    },
    addProspectTMBComponent(value) {
      this.tmb.push({
        id: value.id,
        name: value.name,
        product: [
          {
            product_id: value.id,
            product_name: value.name,
            component: null,
            market_share: null,
            remark: null,
            maintenance_id: null,
          },
        ],
      })
    },
    addProspectTMBIGTE(value) {
      this.tmb.push({
        id: value.id,
        name: value.name,
        product: [
          {
            product_id: value.id,
            product_name: value.name,
            igte: null,
            market_share: null,
            remark: null,
            maintenance_id: null,
          },
        ],
      })
    },
    addProspectTMBLearning(value) {
      this.tmb.push({
        id: value.id,
        name: value.name,
        product: [
          {
            product_id: value.id,
            product_name: value.name,
            learning: null,
            market_share: null,
            remark: null,
            maintenance_id: null,
          },
        ],
      })
    },
    addTMB(data_index, data) {
      this.tmb[data_index].product.push({
        product_id: data.id,
        aircraft_type: null,
        market_share: null,
        remark: null,
        maintenance_id: null,
      })
    },
    addPBTH(product_value, acType_value) {
      this.pbth.push({
        product_id: product_value.id,
        product_name: product_value.name,
        aircraft_type_id: acType_value.id,
        aircraft_type_name: acType_value.name,
        market_share: 0,
        target: [
          {
            month: 'January',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'February',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'March',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'April',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'May',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'June',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'July',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'August',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'September',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'October',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'November',
            rate: null,
            flight_hour: null,
          },
          {
            month: 'December',
            rate: null,
            flight_hour: null,
          },
        ],
      })
    },
    removeProspectTMB(data_index) {
      this.tmb.splice(data_index, 1)
    },
    removeTMB(data_index, item_index) {
      this.tmb[data_index].product.splice(item_index, 1)
      if (this.tmb[data_index].product == '') {
        this.tmb.splice(data_index, 1)
      }

      this.clearError()
    },
    removePBTH(index_pbth) {
      this.pbth.splice(index_pbth, 1)
      this.clearError()
    },
    listCustomer() {
      this.$axios
        .get('api/customer-group', {
          params: {
            hasAms: true,
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.customer_options = response.data.data.data
        })
    },
    listAPU() {
      this.$axios
        .get('api/apu', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.apu_options = response.data.data.data
        })
    },
    listEngine() {
      this.$axios
        .get('api/engine', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.engine_options = response.data.data.data
        })
    },
    listIGTE() {
      this.$axios.get('api/igte').then((response) => {
        this.igte_options = response.data.data
      })
    },
    listLearning() {
      this.$axios.get('api/learning').then((response) => {
        this.learning_options = response.data.data
      })
    },
    listComponent() {
      this.$axios
        .get('api/component', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.component_options = response.data.data.data
        })
    },
    listAcType() {
      this.$axios
        .get('api/aircraft-type', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.acType_options = response.data.data.data
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
          this.maintenance_options = response.data.data.data
        })
    },
    listAmsCustomer({ id }) {
      this.selected_amsCustomer = null
      this.$axios.get(`api/ams-customer/${id}`).then((response) => {
        this.amsCustomer_options = response.data.data
      })
    },
    listStrategicInitiative() {
      this.$axios
        .get('api/strategic-initiative', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.strategic_initiative_options = response.data.data.data
        })
    },
    listUser() {
      this.$axios
        .get('api/users', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.project_manager_options = response.data.data.data
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
          this.product_options = response.data.data.data
        })
    },
    listAircraftType() {
      this.$axios
        .get('api/aircraft-type', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.aircraft_type_options = response.data.data.data
        })
    },
    listProspectType() {
      this.$axios
        .get('api/prospect-type', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.prospect_type = response.data.data
        })
    },
    listProspect(paginate) {
      this.loading()
      paginate = paginate || `/api/prospect`
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
          // Data Prospect
          this.prospect = response.data.data.prospect.data
          console.log(this.prospect)
          // Data Total Market share, Total salesplan & Deviation
          this.prospect2 = response.data.data
          // Pagination
          this.prospect3 = response.data.data.prospect
          // Total Deviation
          this.totalDeviation = response.data.data.deviation
          // Current Page
          this.current_page = this.prospect3.current_page
          Swal.close()
        })
    },
    filterYear(year) {
      this.year = year
      this.listProspect()
    },
    filterYearDefault(year) {
      this.year = null
      this.listProspect()
    },
    listTransactionType() {
      this.loading()
      this.$axios
        .get('api/transaction-type', {
          params: {
            order: 'created_at',
            by: 'ASC',
          },
        })
        .then((response) => {
          this.transaction_type = response.data.data
          Swal.close()
        })
        .catch((error) => console.log(error))
    },
    directPage: debounce(function () {
      if (this.current_page < 1) {
        this.current_page = 1
      } else if (this.current_page > this.prospect3.last_page) {
        this.current_page = this.prospect3.last_page
      }
      let url = new URL(this.prospect3.first_page_url)
      let search_params = url.searchParams
      search_params.set('page', this.current_page)
      url.search = search_params.toString()
      let new_url = url.toString()
      this.listProspect(new_url)
    }, 500),
    createTMBOrganic() {
      this.loading()
      this.$axios
        .post('/api/prospect-create', {
          prospect_type_id: this.prospect_prospect_type_id,
          year: this.prospect_year,
          ams_customer_id: this.selected_amsCustomer.id,
          transaction_type_id: this.prospect_transaction_type_id,
          tmb: this.tmb,
        })
        .then((response) => {
          toastr.success(response.data.message)
          this.closeModal()
          this.listProspect()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          } else {
            toastr.error(error.response.data.message)
          }
        })
    },
    createTMBInorganic() {
      this.loading()
      this.$axios
        .post('/api/prospect-create', {
          prospect_type_id: this.prospect_prospect_type_id,
          year: this.prospect_year,
          ams_customer_id: this.selected_amsCustomer.id,
          transaction_type_id: this.prospect_transaction_type_id,
          strategic_initiative_id: this.selected_strategic_initiative.id,
          pm_id: this.selected_pm.id,
          tmb: this.tmb,
        })
        .then((response) => {
          this.closeModal()
          toastr.success(response.data.message)
          this.listProspect()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          } else {
            toastr.error(error.response.data.message)
            this.$router.push('/login')
          }
        })
    },
    createPBTHOrganic() {
      this.loading()
      this.$axios
        .post('/api/prospect-create', {
          prospect_type_id: this.prospect_prospect_type_id,
          year: this.prospect_year,
          ams_customer_id: this.selected_amsCustomer.id,
          transaction_type_id: this.prospect_transaction_type_id,
          pbth: this.pbth,
        })
        .then((response) => {
          this.closeModal()
          toastr.success(response.data.message)
          this.listProspect()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          } else {
            toastr.error(error.response.data.message)
            this.$router.push('/login')
          }
        })
    },
    createPBTHInorganic() {
      this.loading()
      this.$axios
        .post('/api/prospect-create', {
          prospect_type_id: this.prospect_prospect_type_id,
          year: this.prospect_year,
          ams_customer_id: this.selected_amsCustomer.id,
          transaction_type_id: this.prospect_transaction_type_id,
          strategic_initiative_id: this.selected_strategic_initiative.id,
          pm_id: this.selected_pm.id,
          pbth: this.pbth,
        })
        .then((response) => {
          this.closeModal()
          toastr.success(response.data.message)
          this.listProspect()
        })
        .catch((error) => {
          if (error.response.status == 422) {
            this.errors = error.response.data.errors
            toastr.error(error.response.data.message)
          } else {
            toastr.error(error.response.data.message)
            this.$router.push('/login')
          }
        })
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
    clearError() {
      this.errors = []
    },
    clearForm() {
      this.prospect_year = null
      this.errors = []
      this.selected_customer = null
      this.selected_amsCustomer = null
      this.selected_strategic_initiative = null
      this.selected_pm = null
      this.product_value = null
      this.acType_value = null
      this.tmb = []
      this.pbth = []
      this.resetStepper()
    },
    closeModal() {
      this.clearForm()
      this.prospect_prospect_type_id = null
      document.getElementById('close_modal').click()
    },
    nextButtonClick() {
      document.getElementById('next_step').click()
    },
    backButtonClick() {
      this.clearError()
      document.getElementById('previous_step').click()
    },
    resetStepper() {
      document.getElementById('previous_step').click()
      document.getElementById('previous_step').click()
      document.getElementById('previous_step').click()
    },
    step() {
      var element = document.querySelector('#kt_stepper_example_vertical')
      var stepper = new KTStepper(element)
      stepper.on('kt.stepper.next', function (stepper) {
        stepper.goNext()
      })
      stepper.on('kt.stepper.previous', function (stepper) {
        stepper.goPrevious()
      })
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
#chart {
  height: 8.5rem;
}
</style>
}}
