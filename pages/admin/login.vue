<template>
  <div>
    <div
      v-show="loading"
      class="is-overlay columns is-centered is-vcentered spinner-overlay"
    >
      <orbit-spinner
        :animation-duration="1000"
        :size="180"
        :color="'#00B89C'"
        class="column is-half"
      />
    </div>
    <div v-show="loading === false" id="login">
      <div class="login-card">
        <div class="card-title">
          <h1>Please Sign In</h1>
        </div>

        <div class="content">
          <form method="POST" action="#">
            <input
              id="email"
              v-model="email"
              type="email"
              name="email"
              title="email"
              placeholder="Email"
              required
              autofocus
            />
            <input
              id="password"
              v-model="password"
              type="password"
              name="password"
              title="password"
              placeholder="Password"
              required
            />

            <!-- <div class="level options">
              <div class="checkbox level-left">
                <input id="checkbox" type="checkbox" class="regular-checkbox" />
                <label for="checkbox"></label>
                <span>Remember me</span>
              </div>

              <a class="btn btn-link level-right" href="#">Forgot Password?</a>
            </div> -->

            <button
              type="submit"
              class="btn btn-primary"
              @click.prevent="loggin"
            >
              Login
            </button>
          </form>
        </div>
      </div>
      <!-- <footer>
        <div class="credit">
          <p>Photo by Jack Cain on Unsplash</p>
        </div>
      </footer> -->
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import OrbitSpinner from '@/components/Spinner/OrbitSpinner'

export default {
  layout: 'login',
  components: {
    OrbitSpinner
  },
  data() {
    return {
      email: '',
      password: '',
      loading: true
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.loading = false
      /// this.$nuxt.$loading.finish()
    })
  },
  methods: {
    ...mapGetters('user', ['isTokenValid']),
    async loggin() {
      const credential = {
        email: this.email,
        password: this.password
      }
      await this.$store.dispatch('user/getToken', credential)
      if (this.isTokenValid) {
        this.$router.push('/admin/dashboard')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
$primary: hsl(171, 100%, 41%);
$grey-darker: hsl(0, 0%, 21%);
$grey-dark: hsl(0, 0%, 29%);
$grey: hsl(0, 0%, 48%);
$grey-light: hsl(0, 0%, 71%);
$grey-lighter: hsl(0, 0%, 86%);

#login {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  // background: #f7f7f7;

  .login-card {
    background: #fff;
    width: 24rem;
    box-shadow: 0 0 7px 0 rgba(0, 0, 0, 0.11);

    .card-title {
      background-color: darken($primary, 5%);
      padding: 2rem;

      h1 {
        color: #fff;
        text-align: center;
        font-size: 1.2rem;
      }
    }

    .content {
      padding: 3rem 2.5rem 5rem;
    }

    #email,
    #password {
      display: block;
      width: 100%;
      font-size: 1rem;
      margin-bottom: 1.75rem;
      padding: 0.25rem 0;
      border: none;
      border-bottom: 1px solid $grey-lighter;
      transition: all 0.5s;

      &:hover {
        border-color: $grey;
      }

      &:active,
      &:focus {
        border-color: $primary;
      }
    }

    .checkbox {
      color: $grey-light;
      font-size: 0.8rem;

      span {
        margin-left: 0.5rem;
      }
    }

    a {
      font-size: 0.8rem;
    }

    .options {
      color: $grey-light;
      margin-bottom: 1.5rem;
    }

    button {
      cursor: pointer;
      font-size: 1.2rem;
      color: $primary;
      border-radius: 4rem;
      display: block;
      width: 100%;
      background: transparent;
      border: 2px solid $primary;
      padding: 0.9rem 0 1.1rem;
      transition: color 0.5s, border-color 0.5s;

      &:hover,
      &:focus {
        color: darken($primary, 10%);
        border-color: darken($primary, 10%);
      }

      &:active {
        transform: translateY(1px);
      }
    }
  }
}

label {
  cursor: pointer;
}

.regular-checkbox {
  display: none;
}

.regular-checkbox + label {
  background-color: #fafafa;
  border: 1px solid $grey-lighter;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  padding: 7px;
  border-radius: 3px;
  display: inline-block;
  position: relative;
}

.regular-checkbox:checked + label {
  background-color: #e9ecee;
}

.regular-checkbox:checked + label:after {
  content: '\2714';
  font-size: 11px;
  position: absolute;
  top: 0;
  left: 3px;
  color: $grey-light;
}

input:focus,
select:focus,
textarea:focus,
button:focus {
  outline: none;
}

footer {
  position: absolute;
  bottom: 1px;
  right: 5px;
  color: #fafafa;
}
.spinner-overlay {
  background-color: #202020;
}
</style>
