<template>
  <div id="app" class="container">
    <nav class="navbar navbar-expand-lg bg-dark" data-bs-theme="dark" v-if="authenticated">
      <div class="container-fluid">
        <div class="collapse navbar-collapse">
          <ul class="navbar-nav me-auto">
            <li class="nav-item">
              <router-link v-if="authenticated" to="/dashboard">Dashboard</router-link>
            </li>
            <li class="nav-item">
              <router-link v-if="authenticated" to="/login" v-on:click.native="logout()" replace>Logout</router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <nav class="navbar navbar-expand-lg bg-dark" data-bs-theme="dark" v-if="!authenticated">
      <div class="container-fluid">
        <div class="collapse navbar-collapse">
          <ul class="navbar-nav me-auto">
            <li class="nav-item">
              <router-link to="/register">Register</router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <router-view @authenticated="setAuthenticated" />
  </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                authenticated: false,
            }
        },
        methods: {
            setAuthenticated(status) {
                this.authenticated = status;
            },
            logout() {
                this.authenticated = false;
                localStorage.removeItem('authData');
            }
        }
    }
</script>
