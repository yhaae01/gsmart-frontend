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
            Dashboard
          </p>
          <!--end::Title-->
        </div>
        <!--end::Page title-->
      </div>
      <!--end::Toolbar container-->
    </div>
    <div class="container-fluid mt-5">
      <div class="card shadow-sm">
        <div
          class="row my-5 mx-2 d-flex justify-content-between align-items-center"
        >
          <div class="col">
            <p class="h2">Chart Sales Plan</p>
          </div>
          <div class="col d-flex justify-content-end">
            <div class="col-3 me-4">
              <multiselect
                v-model="selectedYear"
                :options="yearOptions"
                track-by="value"
                placeholder="Select Year"
                label="text"
                id="year"
                @input="onYearSelect"
              ></multiselect>
            </div>
            <button
              class="btn btn-success"
              type="button"
              @click="exportDashboard()"
            >
              <i class="fa-solid fa-file-excel"></i>
              Export to Excel
            </button>
          </div>
        </div>

        <!-- Chart for TP, TPR, TPC, AM -->
        <div class="card-body">
          <!-- Chart Donut Bar -->
          <section
            v-if="
              role == 'Administrator' ||
              role == 'TP' ||
              role == 'TPR' ||
              role == 'TPC'
            "
          >
            <!-- Chart Header -->
            <div class="row my-2">
              <div class="col-lg-9 col-sm-12"></div>
              <div class="col-lg-3 col-sm-12 d-flex justify-content-end">
                <multiselect
                  v-model="typeChart"
                  :options="typeChartOptions"
                  :allow-empty="false"
                  :searchable="false"
                  :close-on-select="false"
                  :show-labels="false"
                ></multiselect>
              </div>
            </div>
            <!-- End Of Chart Header -->

            <!-- Total Chart -->
            <div class="row my-5" v-if="typeChart == 'Total Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutTotal.chartOptions"
                  :series="chartDonutTotal.series"
                  id="TotalChart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarTotal.chartOptions"
                  :series="chartBarTotal.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Total Chart -->

            <!-- Area Chart -->
            <div class="row my-5" v-if="typeChart == 'Area Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutArea.chartOptions"
                  :series="chartDonutArea.series"
                  id="AreaChart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarArea.chartOptions"
                  :series="chartBarArea.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Area Chart -->

            <!-- Group Chart -->
            <div class="row my-5" v-if="typeChart == 'Group Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutGroup.chartOptions"
                  :series="chartDonutGroup.series"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarGroup.chartOptions"
                  :series="chartBarGroup.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Group Chart -->

            <!-- Product Chart -->
            <div class="row my-5" v-if="typeChart == 'Product Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  id="Product Chart"
                  :options="chartDonutProduct.chartOptions"
                  :series="chartDonutProduct.series"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarProduct.chartOptions"
                  :series="chartBarProduct.series"
                ></VueApexCharts>
              </div>
            </div>

            <!-- Cancel Chart -->
            <div class="row my-5" v-if="typeChart == 'Cancel Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutCancel.chartOptions"
                  :series="chartDonutCancel.series"
                  id="Cancel Chart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarCancel.chartOptions"
                  :series="chartBarCancel.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Cancel Chart -->
          </section>
          <!-- End of Chart Donut Bar -->

          <!-- Role AM -->
          <section class="my-10">
            <div class="row">
              <div class="col-lg-9 col-sm-12"></div>
              <div class="col-lg-3 col-sm-12 d-flex justify-content-end">
                <multiselect
                  v-model="typeChartArea"
                  :options="typeChartAreaOptions"
                  :allow-empty="false"
                  :searchable="false"
                  :close-on-select="false"
                  :show-labels="false"
                ></multiselect>
              </div>
            </div>

            <!-- Area I Chart -->
            <div class="row" v-if="typeChartArea == 'Area I Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutAreaI.chartOptions"
                  :series="chartDonutAreaI.series"
                  id="Area I Chart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarAreaI.chartOptions"
                  :series="chartBarAreaI.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Area I Chart -->

            <!-- Area II Chart -->
            <div class="row" v-if="typeChartArea == 'Area II Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutAreaII.chartOptions"
                  :series="chartDonutAreaII.series"
                  id="Area II Chart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarAreaII.chartOptions"
                  :series="chartBarAreaII.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Area II Chart -->

            <!-- Area III Chart -->
            <div class="row" v-if="typeChartArea == 'Area III Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutAreaIII.chartOptions"
                  :series="chartDonutAreaIII.series"
                  id="Area III Chart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarAreaIII.chartOptions"
                  :series="chartBarAreaIII.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of Area III Chart -->

            <!-- KAM Chart -->
            <div class="row" v-if="typeChartArea == 'KAM Chart'">
              <div class="col-lg-6">
                <VueApexCharts
                  type="donut"
                  :options="chartDonutAreaKam.chartOptions"
                  :series="chartDonutAreaKam.series"
                  id="KAM Chart"
                ></VueApexCharts>
              </div>
              <div class="col-lg-6 duo-chart">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartBarAreaKAM.chartOptions"
                  :series="chartBarAreaKAM.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of KAM Chart -->
          </section>
          <!-- Role AM -->

          <!-- RoFo Chart -->
          <section
            class="my-5"
            v-if="
              role == 'Administrator' ||
              role == 'TP' ||
              role == 'TPR' ||
              role == 'TPC'
            "
          >
            <!-- Chart Header -->
            <div class="row custom-height">
              <div class="col-lg-9 col-sm-12"></div>
              <div class="col-lg-3 col-sm-12 d-flex justify-content-end">
                <multiselect
                  v-model="rofoChart"
                  :options="rofoChartOptions"
                  :allow-empty="false"
                  :searchable="false"
                  :close-on-select="false"
                  :show-labels="false"
                ></multiselect>
              </div>
            </div>
            <!-- End Of Chart Header -->

            <!-- RoFo Total Chart -->
            <div class="row" id="single-chart" v-if="rofoChart == 'RoFo Total'">
              <div class="col-lg-9">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartRofoTotalMonth.chartOptions"
                  :series="chartRofoTotalMonth.series"
                ></VueApexCharts>
              </div>
              <div class="col-lg-3">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartRofoTotalYear.chartOptions"
                  :series="chartRofoTotalYear.series"
                ></VueApexCharts>
              </div>
            </div>

            <!-- End Of RoFo Total Chart -->

            <!-- RoFo Sales Plan Garuda Chart -->
            <div
              class="row"
              id="single-chart"
              v-if="rofoChart == 'RoFo Garuda'"
            >
              <div class="col-lg-9">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartRofoGarudaMonth.chartOptions"
                  :series="chartRofoGarudaMonth.series"
                ></VueApexCharts>
              </div>
              <div class="col-lg-3">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartRofoGarudaYear.chartOptions"
                  :series="chartRofoGarudaYear.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of RoFo Sales Plan Garuda Chart -->

            <!-- RoFo Sales Plan Citilink Chart -->
            <div
              class="row"
              id="single-chart"
              v-if="rofoChart == 'RoFo Citilink'"
            >
              <div class="col-lg-9">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartRofoCitilinkMonth.chartOptions"
                  :series="chartRofoCitilinkMonth.series"
                ></VueApexCharts>
              </div>
              <div class="col-lg-3">
                <VueApexCharts
                  type="bar"
                  height="350"
                  :options="chartRofoCitilinkYear.chartOptions"
                  :series="chartRofoCitilinkYear.series"
                ></VueApexCharts>
              </div>
            </div>
            <!-- End Of RoFo Sales Plan Citilink Chart -->
          </section>
          <!-- End of RoFo Chart -->
        </div>
        <!-- End of Chart for TP, TPR, TPC, AM -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'template',
  name: 'IndexPage',
  data() {
    return {
      role: this.$auth.user.role.name,
      user: this.$auth.user.name,

      selectedYear: null,
      yearOptions: [],

      typeChartOptions: [
        'Total Chart',
        'Area Chart',
        'Group Chart',
        'Product Chart',
        'Cancel Chart',
      ],
      typeChart: null,

      typeChartAreaOptions: [
        'Area I Chart',
        'Area II Chart',
        'Area III Chart',
        'KAM Chart',
      ],
      typeChartArea: null,

      rofoChartOptions: ['RoFo Total', 'RoFo Garuda', 'RoFo Citilink'],
      rofoChart: null,

      // Donut Total
      chartDonutTotal: {
        series: [],
        chartOptions: {
          noData: {
            text: 'Loading...',
          },
          fill: {
            colors: ['#658864', '#7B8FA1'],
          },
          colors: ['#658864', '#7B8FA1'],
          labels: ['RKAP', 'Additional'],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut Area
      chartDonutArea: {
        series: [],
        chartOptions: {
          noData: {
            text: 'Loading...',
          },
          fill: {
            colors: ['#C780FA', '#278EA5', '#FFB26B', '#C69749'],
          },
          colors: ['#C780FA', '#278EA5', '#FFB26B', '#C69749'],
          labels: ['Area 1', 'Area 2', 'Area 3', 'KAM'],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut Group
      chartDonutGroup: {
        series: [44, 55],
        chartOptions: {
          labels: ['GA', 'NGA'],
          fill: {
            colors: ['#A75D5D', '#278EA5'],
          },
          colors: ['#A75D5D', '#278EA5'],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut Product
      chartDonutProduct: {
        series: [],
        chartOptions: {
          labels: [],
          fill: {
            colors: [
              '#B08BBB',
              '#278EA5',
              '#735F32',
              '#C69749',
              '#00e396',
              '#B4A5A5',
              '#801336',
              '#EE4540',
              '#1F4287',
              '#FF8B13',
              '#F273E6',
              '#FDA769',
            ],
          },
          colors: [
            '#B08BBB',
            '#278EA5',
            '#735F32',
            '#C69749',
            '#00e396',
            '#B4A5A5',
            '#801336',
            '#EE4540',
            '#1F4287',
            '#FF8B13',
            '#F273E6',
            '#FDA769',
          ],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                    formatter: function (val) {
                      let total_series = 0
                      for (let i = 0; i < val.config.series.length; i++) {
                        total_series += val.config.series[i]
                      }
                      total_series = Math.round(total_series * 100) / 100
                      return total_series
                    },
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut Cancel
      chartDonutCancel: {
        series: [],
        chartOptions: {
          labels: ['Internal Issue', 'External Issue'],
          fill: {
            colors: ['#C780FA', '#278EA5'],
          },
          colors: ['#C780FA', '#278EA5'],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                    formatter: function (val) {
                      let total_series = 0
                      for (let i = 0; i < val.config.series.length; i++) {
                        total_series += val.config.series[i]
                      }
                      total_series = Math.round(total_series * 100) / 100
                      return total_series
                    },
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },

      // Donut Area I
      chartDonutAreaI: {
        series: [],
        chartOptions: {
          labels: [],
          fill: {
            colors: [
              '#B08BBB',
              '#278EA5',
              '#735F32',
              '#C69749',
              '#00e396',
              '#EE4540',
              '#B4A5A5',
            ],
          },
          colors: [
            '#B08BBB',
            '#278EA5',
            '#735F32',
            '#C69749',
            '#00e396',
            '#EE4540',
            '#B4A5A5',
          ],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                    formatter: function (val) {
                      let total_series = 0
                      for (let i = 0; i < val.config.series.length; i++) {
                        total_series += val.config.series[i]
                      }
                      total_series = Math.round(total_series * 100) / 100
                      return total_series
                    },
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut Area II
      chartDonutAreaII: {
        series: [],
        chartOptions: {
          labels: [],
          fill: {
            colors: [
              '#B08BBB',
              '#278EA5',
              '#735F32',
              '#C69749',
              '#00e396',
              '#B4A5A5',
              '#801336',
            ],
          },
          colors: [
            '#B08BBB',
            '#278EA5',
            '#735F32',
            '#C69749',
            '#00e396',
            '#B4A5A5',
            '#801336',
          ],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                    formatter: function (val) {
                      let total_series = 0
                      for (let i = 0; i < val.config.series.length; i++) {
                        total_series += val.config.series[i]
                      }
                      total_series = Math.round(total_series * 100) / 100
                      return total_series
                    },
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut Area III
      chartDonutAreaIII: {
        series: [],
        chartOptions: {
          labels: [],
          fill: {
            colors: ['#B08BBB', '#278EA5', '#735F32', '#C69749'],
          },
          colors: ['#B08BBB', '#278EA5', '#735F32', '#C69749'],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                    formatter: function (val) {
                      let total_series = 0
                      for (let i = 0; i < val.config.series.length; i++) {
                        total_series += val.config.series[i]
                      }
                      total_series = Math.round(total_series * 100) / 100
                      return total_series
                    },
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },
      // Donut KAM
      chartDonutAreaKam: {
        series: [],
        chartOptions: {
          labels: [],
          fill: {
            colors: ['#C780FA', '#278EA5'],
          },
          colors: ['#C780FA', '#278EA5'],
          chart: {
            type: 'donut',
          },
          plotOptions: {
            pie: {
              donut: {
                labels: {
                  show: true,
                  total: {
                    label: 'MUSD',
                    showAlways: true,
                    show: true,
                    formatter: function (val) {
                      let total_series = 0
                      for (let i = 0; i < val.config.series.length; i++) {
                        total_series += val.config.series[i]
                      }
                      total_series = Math.round(total_series * 100) / 100
                      return total_series
                    },
                  },
                },
              },
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                chart: {
                  width: 200,
                },
                legend: {
                  position: 'bottom',
                },
              },
            },
          ],
        },
      },

      // Total Area
      chartBarTotal: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          chart: {
            type: 'bar',
            stacked: false,

            height: 'auto',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: undefined,
              maxHeight: 120,
            },
          },
          fill: {
            opacity: 1,
          },
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
        },
      },
      // Bar Area
      chartBarArea: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: 'auto',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: undefined,
              maxHeight: 120,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // Bar Group
      chartBarGroup: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: 'auto',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: undefined,
              maxHeight: 120,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // Bar Product
      chartBarProduct: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: '450px',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: false,
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: 150,
              maxHeight: 200,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // Bar Cancel
      chartBarCancel: {
        series: [
          {
            name: 'I',
            data: [],
          },
          {
            name: 'II',
            data: [],
          },
          {
            name: 'III',
            data: [],
          },
          {
            name: 'KAM QG',
            data: [],
          },
          {
            name: 'KAM GA',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: true,

            height: '450px',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            position: 'top',
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [
              'Pricing',
              'Reschedule',
              'Missed Plan',
              'Capacity Capability',
              'Customer Financial',
              'Internal Customer',
            ],
            labels: {
              minHeight: 150,
              maxHeight: 200,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },

      // Bar Area I
      chartBarAreaI: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: '450px',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: 150,
              maxHeight: 200,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // Bar Area II
      chartBarAreaII: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: '450px',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: 150,
              maxHeight: 200,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // Bar Area III
      chartBarAreaIII: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: '450px',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: 150,
              maxHeight: 200,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // Bar KAM
      chartBarAreaKAM: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            stacked: false,

            height: '450px',
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
            labels: {
              minHeight: 150,
              maxHeight: 200,
            },
          },
          fill: {
            opacity: 1,
          },
        },
      },

      // RoFo Total Month
      chartRofoTotalMonth: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          chart: {
            type: 'bar',
            height: 350,
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // RoFo Total Year
      chartRofoTotalYear: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            height: 350,
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
          },
          fill: {
            opacity: 1,
          },
        },
      },

      // RoFo Garuda Month
      chartRofoGarudaMonth: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            height: 350,
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // RoFo Garuda Year
      chartRofoGarudaYear: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            height: 350,
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
          },
          fill: {
            opacity: 1,
          },
        },
      },

      // RoFo Citilink Month
      chartRofoCitilinkMonth: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            height: 350,
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
          },
          fill: {
            opacity: 1,
          },
        },
      },
      // RoFo Citilink Year
      chartRofoCitilinkYear: {
        series: [
          {
            name: 'Target',
            data: [],
          },
          {
            name: 'Progress',
            data: [],
          },
        ],
        chartOptions: {
          grid: {
            xaxis: {
              lines: {
                show: false,
              },
            },
            yaxis: {
              lines: {
                show: false,
              },
            },
          },
          chart: {
            type: 'bar',
            height: 350,
            toolbar: {
              show: false,
            },
          },
          plotOptions: {
            bar: {
              horizontal: false,
              columnWidth: '55%',
              endingShape: 'rounded',
            },
          },
          dataLabels: {
            enabled: true,
            enabledOnSeries: undefined,
            formatter: function (val, opts) {
              return val
            },
            textAnchor: 'middle',
            distributed: false,
            offsetX: 0,
            offsetY: 0,
            style: {
              fontSize: '11px',
              fontWeight: 'bold',
              colors: undefined,
            },
            background: {
              enabled: true,
              foreColor: '#fff',
              padding: 4,
              borderRadius: 2,
              borderWidth: 1,
              borderColor: '#fff',
              opacity: 0.9,
              dropShadow: {
                enabled: false,
                top: 1,
                left: 1,
                blur: 1,
                color: '#000',
                opacity: 0.45,
              },
            },
            dropShadow: {
              enabled: false,
              top: 1,
              left: 1,
              blur: 1,
              color: '#000',
              opacity: 0.45,
            },
          },
          stroke: {
            show: true,
            width: 2,
            colors: ['transparent'],
          },
          xaxis: {
            categories: [],
          },
          fill: {
            opacity: 1,
          },
        },
      },
    }
  },
  created() {
    this.loading()
    this.totalChart()
    this.areaChart()
    this.groupChart()
    this.productChart()
    this.cancelChart()

    this.areaIChart()
    this.areaIIChart()
    this.areaIIIChart()
    this.kamChart()

    this.rofoTotal()
    this.rofoTotalYear()
    this.rofoTotalGaruda()
    this.rofoTotalGarudaYear()
    this.rofoTotalCitilink()
    this.rofoTotalCitilinkYear()
  },
  mounted() {
    this.generateYearOptions()
  },
  methods: {
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
    exportDashboard() {
      Swal.fire({
        timer: 1500,
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
      })

      this.$axios
        .get('/api/dashboard-export', {
          responseType: 'blob',
        })
        .then((response) => {
          const tempLink = document.createElement('a')
          tempLink.href = window.URL.createObjectURL(new Blob([response.data]))
          tempLink.setAttribute('download', 'Sales_Report.xlsx')
          document.body.appendChild(tempLink)
          tempLink.click()

          setTimeout(() => {
            document.body.removeChild(tempLink)
          }, 500)
        })
        .then(() => {
          toastr.success('Downloading Excel Sales Report')
        })
        .then((error) => console.log(error))
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
      this.loading()

      this.totalChart()
      this.areaChart()
      this.groupChart()
      this.productChart()
      this.cancelChart()

      this.areaIChart()
      this.areaIIChart()
      this.areaIIIChart()
      this.kamChart()

      this.rofoTotal()
      this.rofoTotalYear()
      this.rofoTotalGaruda()
      this.rofoTotalGarudaYear()
      this.rofoTotalCitilink()
      this.rofoTotalCitilinkYear()
    },

    totalChart() {
      this.loading()

      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-total', { params })
        .then((response) => {
          // TotalChart Update Series
          this.chartDonutTotal.series = response.data.data.pie

          // Check if the response has bar data
          if (response.data.data.bar) {
            this.chartBarTotal.series[0].data =
              response.data.data.bar.target || []
            this.chartBarTotal.series[1].data =
              response.data.data.bar.progress || []
            this.chartBarTotal.chartOptions.xaxis.categories =
              response.data.data.bar.percentage || []
          } else {
            // If no bar data, set bar data to empty
            this.chartBarTotal.series[0].data = []
            this.chartBarTotal.series[1].data = []
            this.chartBarTotal.chartOptions.xaxis.categories = []
          }
        })
        .catch((error) => console.log(error))
    },

    areaChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-area', { params })
        .then((response) => {
          // areaChart Update Series
          this.chartDonutArea.series = response.data.data.pie

          this.chartBarArea.series[0].data = response.data.data.bar.target
          this.chartBarArea.series[1].data = response.data.data.bar.progress
          this.chartBarArea.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },
    groupChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-group', { params })
        .then((response) => {
          // groupChart Update Series
          this.chartDonutGroup.series = response.data.data.pie

          this.chartBarGroup.series[0].data = response.data.data.bar.target
          this.chartBarGroup.series[1].data = response.data.data.bar.progress
          this.chartBarGroup.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },
    productChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-product', { params })
        .then((response) => {
          // productChart Update Series
          this.chartDonutProduct.series = response.data.data.pie.target
          this.chartDonutProduct.chartOptions.labels =
            response.data.data.pie.label

          this.chartBarProduct.series[0].data = response.data.data.bar.target
          this.chartBarProduct.series[1].data = response.data.data.bar.progress
          this.chartBarProduct.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },
    cancelChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-cancel', { params })
        .then((response) => {
          // cancelChart Update Series
          this.chartDonutCancel.series = response.data.data.pie

          this.chartBarCancel.series[0].data = response.data.data.bar[0].data
          this.chartBarCancel.series[1].data = response.data.data.bar[1].data
          this.chartBarCancel.series[2].data = response.data.data.bar[2].data
          this.chartBarCancel.series[3].data = response.data.data.bar[3].data
          this.chartBarCancel.series[4].data = response.data.data.bar[4].data

          this.typeChart = 'Total Chart'
        })
        .catch((error) => console.log(error))
    },

    // AM Chart
    areaIChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-ams-area-1', { params })
        .then((response) => {
          // Area I Chart Update Series
          this.chartDonutAreaI.series = response.data.data.pie.target
          this.chartDonutAreaI.chartOptions.labels =
            response.data.data.pie.label

          this.chartBarAreaI.series[0].data = response.data.data.bar.target
          this.chartBarAreaI.series[1].data = response.data.data.bar.progress
          this.chartBarAreaI.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },
    areaIIChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-ams-area-2', { params })
        .then((response) => {
          // Area II Chart Update Series
          this.chartDonutAreaII.series = response.data.data.pie.target
          this.chartDonutAreaII.chartOptions.labels =
            response.data.data.pie.label

          this.chartBarAreaII.series[0].data = response.data.data.bar.target
          this.chartBarAreaII.series[1].data = response.data.data.bar.progress
          this.chartBarAreaII.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },
    areaIIIChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-ams-area-3', { params })
        .then((response) => {
          // Area III Chart Update Series
          this.chartDonutAreaIII.series = response.data.data.pie.target
          this.chartDonutAreaIII.chartOptions.labels =
            response.data.data.pie.label

          this.chartBarAreaIII.series[0].data = response.data.data.bar.target
          this.chartBarAreaIII.series[1].data = response.data.data.bar.progress
          this.chartBarAreaIII.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },
    kamChart() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-ams-area-kam', { params })
        .then((response) => {
          // Area III Chart Update Series
          this.chartDonutAreaKam.series = response.data.data.pie.target
          this.chartDonutAreaKam.chartOptions.labels =
            response.data.data.pie.label

          this.chartBarAreaKAM.series[0].data = response.data.data.bar.target
          this.chartBarAreaKAM.series[1].data = response.data.data.bar.progress
          this.chartBarAreaKAM.chartOptions.xaxis.categories =
            response.data.data.bar.percentage
        })
        .catch((error) => console.log(error))
    },

    // RoFo
    rofoTotal() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-rofo-total-month', { params })
        .then((response) => {
          // rofoTotal Update Series
          this.chartRofoTotalMonth.series[0].data = response.data.data.target
          this.chartRofoTotalMonth.series[1].data = response.data.data.progress
          this.chartRofoTotalMonth.chartOptions.xaxis.categories =
            response.data.data.percentage
        })
        .catch((error) => console.log(error))
    },
    rofoTotalYear() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-rofo-total-year', { params })
        .then((response) => {
          // chartRofoTotalYear Update Series
          this.chartRofoTotalYear.series[0].data = response.data.data.target
          this.chartRofoTotalYear.series[1].data = response.data.data.progress
          this.chartRofoTotalYear.chartOptions.xaxis.categories =
            response.data.data.percentage

          this.typeChartArea = 'Area I Chart'
          this.rofoChart = 'RoFo Total'
        })
        .catch((error) => console.log(error))
    },
    rofoTotalGaruda() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-rofo-garuda-month', { params })
        .then((response) => {
          // rofoTotalGaruda Update Series
          this.chartRofoGarudaMonth.series[0].data = response.data.data.target
          this.chartRofoGarudaMonth.series[1].data = response.data.data.progress
          this.chartRofoGarudaMonth.chartOptions.xaxis.categories =
            response.data.data.percentage
        })
        .catch((error) => console.log(error))
    },
    rofoTotalGarudaYear() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-rofo-garuda-year', { params })
        .then((response) => {
          // chartRofoGarudaYear Update Series
          this.chartRofoGarudaYear.series[0].data = response.data.data.target
          this.chartRofoGarudaYear.series[1].data = response.data.data.progress
          this.chartRofoGarudaYear.chartOptions.xaxis.categories =
            response.data.data.percentage

          this.rofoChart = 'RoFo Total'
        })
        .catch((error) => console.log(error))
    },
    rofoTotalCitilink() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-rofo-citilink-month', { params })
        .then((response) => {
          // rofoTotalCitilink Update Series
          this.chartRofoCitilinkMonth.series[0].data = response.data.data.target
          this.chartRofoCitilinkMonth.series[1].data =
            response.data.data.progress
          this.chartRofoCitilinkMonth.chartOptions.xaxis.categories =
            response.data.data.percentage
        })
        .catch((error) => console.log(error))
    },
    rofoTotalCitilinkYear() {
      const params = {
        year: this.selectedYear ? this.selectedYear.value : null,
      }

      this.$axios
        .get('api/dashboard-rofo-citilink-year', { params })
        .then((response) => {
          // Chart 12 Update Series
          this.chartRofoCitilinkYear.series[0].data = response.data.data.target
          this.chartRofoCitilinkYear.series[1].data =
            response.data.data.progress
          this.chartRofoCitilinkYear.chartOptions.xaxis.categories =
            response.data.data.percentage
        })
        .catch((error) => console.log(error))
    },
  },
}
</script>
<style>
.center-chart {
  margin: auto;
}
.mt-20 {
  margin-top: 20px;
}
.mb-20 {
  margin-bottom: 20px;
}
.custom-height {
  min-height: 50px;
}
.custom-rofo {
  min-height: 50vh;
}
.single-chart {
  max-width: 1080px;
  margin: auto;
}
.duo-chart {
  max-width: 720px;
  margin: auto;
}
</style>
