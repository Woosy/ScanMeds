<template>
  <div class="roboto">
    <div id="wrapper">
      <!-- ============================== -->
      <!-- == SIDEBAR =================== -->
      <!-- ============================== -->

      <ul
        id="accordionSidebar"
        class="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion"
      >
        <!-- Sidebar - Brand -->
        <a
          class="sidebar-brand d-flex align-items-center"
          href="/#/dashboard?page=dashboard"
        >
          <div class="sidebar-brand-icon text-left ml-2 mr-2">
            <img
              class="img-fluid"
              style="width: 2rem"
              src="../assets/logo.png"
              alt="Logo"
            >
          </div>
          <div class="sidebar-brand-text">ScanMeds</div>
        </a>

        <hr class="sidebar-divider my-0">

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'dashboard' } ]"
            class="nav-link"
            href="/#/dashboard?page=dashboard"
          >
            <i
              class="fas fa-fw fa-tachometer-alt"
              :class="['here', { active: page === 'dashboard' } ]"
            />
            <span>Tableau de bord</span>
          </a>
        </li>

        <hr class="sidebar-divider">

        <div class="sidebar-heading">
          Médicaments
        </div>

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'search' } ]"
            class="nav-link"
            href="/#/dashboard?page=search"
          >
            <i
              class="fas fa-fw fa-search"
              :class="['here', { active: page === 'search' } ]"
            />
            <span>Rechercher</span></a>
        </li>

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'favs' } ]"
            class="nav-link"
            href="/#/dashboard?page=favs"
          >
            <i
              class="fas fa-fw fa-heart"
              :class="['here', { active: page === 'favs' } ]"
            />
            <span>Médicaments favoris</span></a>
        </li>

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'recents' } ]"
            class="nav-link"
            href="/#/dashboard?page=recents"
          >
            <i
              class="fas fa-fw fa-table"
              :class="['here', { active: page === 'recents' } ]"
            />
            <span>Recherches récentes</span></a>
        </li>

        <hr class="sidebar-divider">

        <div class="sidebar-heading">
          Mon espace santé
        </div>

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'profile' } ]"
            class="nav-link"
            href="/#/dashboard?page=profile"
          >
            <i
              class="fas fa-fw fa-user"
              :class="['here', { active: page === 'profile' } ]"
            />
            <span>Informations</span></a>
        </li>

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'appointments' } ]"
            class="nav-link"
            href="/#/dashboard?page=appointments"
          >
            <i
              class="fas fa-fw fa-address-book"
              :class="['here', { active: page === 'appointments' } ]"
            />
            <span>Rendez-vous</span></a>
        </li>

        <hr class="sidebar-divider">

        <div class="sidebar-heading">
          Autres
        </div>

        <li class="nav-item">
          <a
            :class="['here', { active: page === 'settings' } ]"
            class="nav-link"
            href="/#/dashboard?page=settings"
          >
            <i
              class="fas fa-fw fa-cogs"
              :class="['here', { active: page === 'settings' } ]"
            />
            <span>Paramètres</span></a>
        </li>

        <hr class="sidebar-divider">

        <!-- Sidebar Toggler (Sidebar) -->
        <div class="text-center d-none d-md-inline">
          <button
            id="sidebarToggle"
            class="rounded-circle border-0"
            @click="toggleSidebar"
          />
        </div>
      </ul>

      <div
        id="content-wrapper"
        class="d-flex flex-column"
      >
        <!-- ============================== -->
        <!-- == NAVBAR ==================== -->
        <!-- ============================== -->

        <nav class="navbar navbar-expand navbar-light bg-white topbar mb-4 static-top shadow">
          <!-- Sidebar Toggle (Topbar) -->
          <button
            id="sidebarToggleTop"
            class="btn btn-link d-md-none rounded-circle mr-3"
            @click="toggleSidebar"
          >
            <i class="fa fa-bars" />
          </button>

          <!-- Topbar Navbar -->
          <ul class="navbar-nav ml-auto">
            <!-- Nav Item - Alerts -->
            <li class="nav-item dropdown no-arrow mx-1">
              <a
                id="alertsDropdown"
                class="nav-link dropdown-toggle"
                href="#"
                role="button"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
              >
                <i class="fas fa-bell fa-fw" />
                <!-- Counter - Alerts -->
                <span class="badge badge-danger badge-counter">{{ notifications && notifications.length ? `${notifications.length}+` : '' }}</span>
              </a>
              <!-- Dropdown - Alerts -->
              <div
                class="dropdown-list dropdown-menu dropdown-menu-right shadow animated--grow-in"
                aria-labelledby="alertsDropdown"
              >
                <h6 class="dropdown-header">
                  Notifications
                </h6>

                <div v-if="notifications && notifications.length">
                  <a
                    v-for="(notification, index) in notifications"
                    :key="index"
                    class="dropdown-item d-flex align-items-center"
                    @click="readNotification(notification._id)"
                  >
                    <div class="mr-3">
                      <div class="icon-circle bg-primary">
                        <i class="fas fa-file-alt text-white" />
                      </div>
                    </div>
                    <div>
                      <div class="font-weight-bold text-gray-700">{{ notification.title }}</div>
                      <span class="small text-gray-600">{{ notification.message }}</span>
                    </div>
                  </a>

                  <a
                    class="dropdown-item text-center small text-gray-500"
                    @click="readAllNotifications()"
                  >Marquer tout comme "lu"</a>
                </div>

                <div
                  v-if="!notifications || !notifications.length"
                  class="text-center text-gray-700 mb-3 mt-3"
                >
                  <span>Aucune notification !</span>
                </div>
              </div>
            </li>

      

            <div class="topbar-divider d-none d-sm-block" />

            <!-- Nav Item - User Information -->
            <li class="nav-item dropdown no-arrow">
              <a
                id="userDropdown"
                class="nav-link dropdown-toggle"
                role="button"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
              >
                <span class="mr-2 d-none d-lg-inline text-gray-600 small">{{ user.firstame && user.lastname ? `${user.firstname} ${user.lastname}` : user.username }}</span>
                <img
                  class="img-profile rounded-circle"
                  src="https://www.shareicon.net/data/128x128/2016/05/24/770121_man_512x512.png"
                >
              </a>
              <!-- Dropdown - User Information -->
              <div
                class="dropdown-menu dropdown-menu-right shadow animated--grow-in"
                aria-labelledby="userDropdown"
              >
                <a
                  class="dropdown-item text-gray-500"
                  href="/#/dashboard?page=profile"
                >
                  <i class="fas fa-user fa-sm fa-fw mr-2" />
                  Mon profil
                </a>
                <a
                  class="dropdown-item text-gray-500"
                  href="/#/dashboard?page=settings"
                >
                  <i class="fas fa-cogs fa-sm fa-fw mr-2 " />
                  Paramètres
                </a>
                <div class="dropdown-divider" />
                <a
                  class="dropdown-item text-gray-500"
                  data-toggle="modal"
                  data-target="#logoutModal"
                  @click="logout"
                >
                  <i class="fas fa-sign-out-alt fa-sm fa-fw mr-2" />
                  Déconnexion
                </a>
              </div>
            </li>
          </ul>
        </nav>


        <!-- ============================== -->
        <!-- == PAGES ===================== -->
        <!-- ============================== -->

        <div id="content">
          <div class="container-fluid">
            <!-- display page -->

            <Main
              v-show="page === 'dashboard'"
              :notifications="notifications"
              :favorites="favorites"
            />
            <Settings v-show="page === 'settings'" />
            <Search
              v-show="page === 'search'"
              :favorites="favorites"
            />
            <Favs
              v-show="page === 'favs'"
              :favorites="favorites"
            />
            <Recents v-show="page === 'recents'" />
            <Profile v-show="page === 'profile'" />
            <Appointments v-show="page === 'appointments'" />
          </div>
        </div>

        <Footer />
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import router from '../router'

import Footer from '../components/Footer.vue'

import Main from '../components/pages/Main.vue'
import Settings from '../components/pages/Settings.vue'
import Search from '../components/pages/Search.vue'
import Favs from '../components/pages/Favs.vue'
import Recents from '../components/pages/Recents.vue'
import Profile from '../components/pages/Profile.vue'
import Appointments from '../components/pages/Appointments.vue'

export default {
  name: 'Dashboard',
  components: {
    Footer,
    Main,
    Settings,
    Search,
    Favs,
    Recents,
    Profile,
    Appointments
  },
  data() {
    return {
      page: this.$route.query.page,
      notifications: [],
      favorites: []
    }
  },
  computed: {
    user: () => {
      return JSON.parse(localStorage.getItem('user'))
    }
  },
  watch: {
    '$route' (to, from) {
      this.page = to.query.page
    }
  },
  mounted () {
    // if user's not logged in, redirect to login page
    if (!localStorage.getItem('token')) {
      router.push('/')
    }

    // get user's notifications
    this.$http.get(`${this.$apiUrl}/notification/notifications`).then(res => {
      this.notifications = res.data.notifications
    })

    // get user's favorites medicines
    this.$http.get(`${this.$apiUrl}/favorite/favorites`).then(res => {
      this.favorites = res.data.favorites  
    })
  },
  methods: {
    logout: function () {
      localStorage.removeItem('token')
      localStorage.removeItem('user')
      router.push('/')
    },
    toggleSidebar: function () {
      document.querySelector('body').classList.toggle('sidebar-toggled')
      document.querySelector('.sidebar').classList.toggle('toggled')
    },
    readNotification: function (notificationId) {
      this.$http.patch(`${this.$apiUrl}/notification/read/${notificationId}`).then(res => {
        // remove the notification from the front, without having to call the api
        this.notifications = this.notifications.filter(notification => notification._id !== notificationId)
      })
    },
    readAllNotifications: function () {
      this.$http.patch(`${this.$apiUrl}/notification/read/all`).then(res => {
        // remove the notification from the front, without having to call the api
        this.notifications = []
      })
    }
  }
}
</script>

<style scoped>
.here.active {
  color: white !important;
}

.roboto {
  font-family: 'Roboto', sans-serif !important;
}
</style>