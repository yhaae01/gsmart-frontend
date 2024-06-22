<template>
  <div class="row justify-content-center ms-0 me-0">
    <div class="col-md-9 col-sm-12 px-0">
      <div class="container">
        <img
          src="~/assets/media/logos/gmf-logo.png"
          class="mt-30 logo-size"
          alt="logo"
        />
        <div class="text-white mt-vh">
          <h1 class="text-white fs-large">G-SMART</h1>
          <div class="blok mb-5" />
          <p class="fs-3 mb-1">For Sales & Marketing</p>
          <p class="mb-10">
            if you have any trouble using this application, please contact us at
            spoc@gmf-aeroasia.co.id
          </p>
        </div>
      </div>
    </div>
    <div
      class="col-sm-12 col-md-3 bg-color d-flex align-items-center justify-content-center"
    >
      <div class="card">
        <div class="card-body px-30">
          <h1 class="fw-bold mb-50 text-center top-margin">
            Login to your account.
          </h1>

          <form method="post" v-on:submit.prevent="loginUser">
            <div
              class="form-group mt-5"
              :class="{ 'has-error': errors.username }"
            >
              <input
                type="text"
                class="form-control rounded-pill"
                placeholder="Employee Number"
                v-model="login.username"
                @input="validateUsername"
              />
            </div>
            <div class="text-center mx-c">
              <p v-if="errors.username" class="text-danger mb-0 ms-2 mt-2">
                {{ errors.username }}
              </p>
            </div>
            <div
              class="form-group mt-10"
              :class="{ 'has-error': errors.password }"
            >
              <input
                type="password"
                class="form-control rounded-pill"
                placeholder="Password"
                v-model="login.password"
                @input="validatePassword"
              />
            </div>
            <div class="text-center mx-c">
              <p v-if="errors.password" class="text-danger mb-0 ms-2 mt-2">
                {{ errors.password }}
              </p>
            </div>
            <div class="mt-10 d-flex justify-content-center mb-85">
              <button
                type="submit"
                class="btn btn-success rounded-pill w-200px"
              >
                Login
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginPage',
  head() {
    return {
      bodyAttrs: {
        class: 'bg-background',
      },
    }
  },
  data() {
    return {
      login: {
        username: '',
        password: '',
      },
      errors: {
        username: '',
        password: '',
      },
      submitted: false,
    }
  },

  created() {
    this.validateUsername()
    this.validatePassword()
  },

  methods: {
    validateUsername() {
      if (this.submitted) {
        this.errors.username = this.login.username
          ? ''
          : 'Please insert a valid username!'
      }
    },
    validatePassword() {
      if (this.submitted) {
        this.errors.password = this.login.password
          ? ''
          : 'Please insert a valid password!'
      }
    },
    async loginUser() {
      this.submitted = true

      this.errors = {
        username: '',
        password: '',
      }

      await this.$auth
        .loginWith(
          'local',
          {
            data: this.login,
          },
          this.loading()
        )
        .then((result) => {
          this.$router.push('/')
          this.successMessage()
        })
        .catch((error) => {
          if (error.response.status == 401) {
            this.errors.username = 'Username is invalid!'
            this.errors.password = 'Password is invalid!'
            this.failMessage()
          } else if (error.response.status == 422) {
            this.errors.username = 'Please insert a valid username!'
            this.errors.password = 'Please insert a valid password!'
            this.failMessage()
          }
        })
    },
    loading() {
      Swal.fire({
        timer: 5000,
        didOpen: () => {
          Swal.showLoading()
        },
        background: 'transparent',
        allowOutsideClick: false,
      })
    },
    successMessage() {
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
      toastr.success('Login Successfully!')
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
      toastr.error('Login failed, please try again!')
    },
  },
}
</script>

<style>
.has-error input {
  outline-color: red;
  border-color: red;
}
</style>
