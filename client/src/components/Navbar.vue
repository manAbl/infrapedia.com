<template>
  <header class="header fixed h12 w-full no-padding">
    <div
      class="header-inner-wrapper w-full flex relative h-fit-full bottom-shadow"
    >
      <div
        class="flex no-padding navbar-wrapper w-full justify-content-space-between pr1 pl1"
      >
        <h1 class="logo-title hidden-md-and-down">
          <router-link to="/">
            <el-image class="mt2 logo-img" :src="imageURL" fit="scale-down" />
          </router-link>
        </h1>

        <h1>
          <i-mobile-drawer
            v-if="!isUserNavbar"
            class="hidden-md-and-up"
            @click-list-item="handleBeforeLoadItem"
          />
          <i-mobile-drawer-profile class="hidden-lg-and-up" v-else />
        </h1>

        <div aria-labelledby="rightnavheading" class="links-wrapper">
          <transition-group
            tag="div"
            class="h-fit-full"
            name="fade"
            mode="out-in"
          >
            <ul
              class="p0 m0 h-fit-full"
              role="group"
              :key="1"
              v-if="!isUserNavbar"
            >
              <li
                class="inline-block no-selectable relative hidden-sm-and-down"
                role="listitem"
              >
                <premium-partners-button
                  @item-selected="handleBeforeLoadItem"
                />
              </li>

              <el-divider direction="vertical" class="m0 hidden-sm-and-down" />

              <li
                class="inline-block relative hidden-sm-and-down"
                data-no-outline="true"
                @click="toggleDrawerVisibility"
                @keyup.enter="toggleDrawerVisibility"
              >
                <div class="list-item pr4 pl4 no-selectable">
                  <bottom-sheet
                    :visibility="isDrawerOpen"
                    @close="toggleDrawerVisibility"
                    @item-selected="handleBeforeLoadItem"
                  />
                </div>
              </li>

              <li
                class="inline-block relative hidden-sm-and-down"
                data-no-outline="true"
                role="listitem"
              >
                <div
                  class="list-item info-menu pr1 pl1"
                  aria-haspopup="true"
                  data-no-hover-bg="true"
                >
                  <i-search @search-selection="handleBeforeLoadItem" />
                </div>
              </li>

              <i-full-screen-search @search-selection="handleBeforeLoadItem" />

              <li
                class="inline-block relative"
                data-no-outline="true"
                role="listitem"
              >
                <div
                  class="list-item info-menu pr2 pl2"
                  aria-haspopup="true"
                  data-no-hover-bg="true"
                >
                  <i-filter @open="closeUnwantedOpenMenus" />
                </div>
              </li>

              <li
                class="inline-block hidden-sm-and-down relative"
                data-no-outline="true"
                role="listitem"
              >
                <div class="list-item" data-no-hover-bg="true">
                  <a href="https://www.catchpoint.com" target="_blank">
                    <el-image
                      src="https://storage.googleapis.com/infrapedia_bucket/sponsors/catchpoint-logo.png"
                      lazy
                      class="w28 image-sponsor"
                      fit="cover"
                      alt="catchpoint logo"
                      referrer-policy="strict-origin-when-cross-origin"
                    />
                  </a>
                </div>
              </li>

              <el-divider direction="vertical" class="m0" />

              <li
                class="inline-block hidden-sm-and-down relative"
                data-no-outline="true"
                role="listitem"
              >
                <div class="list-item" data-no-hover-bg="true">
                  <a href="https://ipv4.global" target="_blank">
                    <el-image
                      lazy
                      src="https://storage.googleapis.com/infrapedia_bucket/sponsors/ipv4global-footer.png"
                      fit="center"
                      class="w28 image-sponsor ipv4"
                      alt="ipv4 logo"
                      referrer-policy="strict-origin-when-cross-origin"
                    />
                  </a>
                </div>
              </li>

              <li class="inline-block relative" data-no-outline="true">
                <div
                  class="list-item user-icon pr1 pl3"
                  aria-haspopup="true"
                  data-no-hover-bg="true"
                >
                  <el-popover
                    :visible-arrow="false"
                    placement="bottom-end"
                    width="220"
                    :popper-class="popoverClassGiver"
                    transition="el-zoom-in-top"
                    trigger="manual"
                    v-model="isUserMenuOpen"
                  >
                    <template v-if="isProfileRoute">
                      <header
                        class="header no-selectable h8 flex justify-content-space-between pr7 pl6 pt2 pb2 align-items-center"
                        :class="{ dark, light: !dark }"
                      >
                        <strong class="fs-regular truncate">{{
                          userName
                        }}</strong>
                        <span
                          class="fs-regular cursor-pointer"
                          @click="toggleUserMenuVisibility"
                        >
                          <fa :icon="['fas', 'times']" />
                        </span>
                      </header>
                      <ul class="p1 h-fit">
                        <li class="w-fit-full h10">
                          <el-button
                            type="text"
                            class="inline-block color-inherit"
                          >
                            <fa :icon="['fas', 'map']" class="mr4 ml4" />
                            <router-link to="/" class="color-inherit"
                              >Back to map</router-link
                            >
                          </el-button>
                        </li>
                        <li class="w-fit-full h10">
                          <el-button
                            type="text"
                            class="inline-block color-inherit"
                          >
                            <fa
                              :icon="['fas', 'sign-out-alt']"
                              class="mr5 ml4"
                            />
                            <strong>Sign out</strong>
                          </el-button>
                        </li>
                      </ul>
                    </template>
                    <div
                      slot="reference"
                      class="no-outline circle icon-wrapper"
                      tabindex="0"
                      @click="handleUserButtonDynamics"
                      @keyup.enter.space="handleUserButtonDynamics"
                    >
                      <el-avatar
                        v-if="userImage"
                        size="small"
                        :src="$auth.user.picture"
                      />
                      <fa
                        v-else
                        :icon="['fas', 'user-circle']"
                        class="md-icon user-icon"
                      />
                    </div>
                  </el-popover>
                </div>
              </li>
              <li class="inline-block relative" data-no-outline="true">
                <div
                  class="list-item info-menu pr1 pl1"
                  aria-haspopup="true"
                  data-no-hover-bg="true"
                >
                  <div
                    class="w4 pt2 pb1 pr2 pl2 no-outline icon-wrapper circle"
                    tabindex="0"
                    @click.stop="toggleInfoMenuVisibility"
                    @keyup.enter.space="toggleInfoMenuVisibility"
                  >
                    <el-popover
                      :visible-arrow="false"
                      placement="bottom-end"
                      width="200"
                      transition="el-zoom-in-top"
                      trigger="manual"
                      :popper-class="popoverClassGiver"
                      v-model="isInfoMenuOpen"
                    >
                      <ul
                        class="pt4 pr4 pl4 pb0"
                        :class="{ dark, light: !dark }"
                      >
                        <li
                          v-for="(link, i) in infoMenuLinks.info"
                          :key="i"
                          class="w-fit-full h6 mb4 link-info"
                        >
                          <router-link
                            v-if="link.router"
                            :to="link.url"
                            v-text="link.label"
                            class="inline-block w-inherit color-inherit underline-hover"
                          />
                          <a
                            v-else
                            :href="link.url"
                            v-text="link.label"
                            target="_blank"
                            rel="noopener"
                            class="inline-block w-inherit color-inherit underline-hover"
                          />
                        </li>
                      </ul>
                      <el-divider class="m0" />
                      <ul
                        class="flex justify-content-space-around pt2 p2"
                        :class="{ dark, light: !dark }"
                      >
                        <li
                          v-for="(link, i) in infoMenuLinks.social"
                          :key="i"
                          class="w4 h4 circle-bg link-info social p2"
                        >
                          <a
                            :href="link.url"
                            target="_blank"
                            rel="noopener"
                            class="w-inherit color-inherit vertical-align"
                          >
                            <fa :icon="link.icon" class="sm-icon" />
                          </a>
                        </li>
                      </ul>
                      <fa
                        slot="reference"
                        class="sm-icon outline0"
                        :icon="['fas', 'ellipsis-v']"
                      />
                    </el-popover>
                  </div>
                </div>
              </li>
            </ul>

            <ul class="p0 pr4 m0 h-fit-full" role="group" :key="2" v-else>
              <li class="inline-block relative" data-no-outline="true">
                <router-link class="list-item pl3 pr3" to="/blog">
                  Blog
                </router-link>
              </li>
              <li class="inline-block relative" data-no-outline="true">
                <div
                  class="list-item user-icon pr1 pl3"
                  aria-haspopup="true"
                  data-no-hover-bg="true"
                >
                  <el-popover
                    :visible-arrow="false"
                    placement="bottom-end"
                    width="220"
                    transition="el-zoom-in-top"
                    trigger="manual"
                    v-model="isUserMenuOpen"
                  >
                    <header
                      class="header no-selectable h8 flex justify-content-space-between pr7 pl6 pt2 pb2 align-items-center"
                      :class="{ dark, light: !dark }"
                    >
                      <strong class="fs-regular truncate">{{
                        userName
                      }}</strong>
                      <span
                        class="fs-regular cursor-pointer"
                        @click="toggleUserMenuVisibility"
                      >
                        <fa :icon="['fas', 'times']" />
                      </span>
                    </header>
                    <ul class="p1 h-fit" :class="{ dark, light: !dark }">
                      <li class="w-fit-full h10">
                        <el-button
                          type="text"
                          class="inline-block color-inherit"
                        >
                          <router-link
                            to="/"
                            class="color-inherit"
                            @click="() => (isUserMenuOpen = false)"
                          >
                            <fa :icon="['fas', 'map']" class="mr4 ml4" />
                            Back to map
                          </router-link>
                        </el-button>
                      </li>
                      <li class="w-fit-full h10">
                        <el-button
                          type="text"
                          class="inline-block color-inherit"
                          @click="toggleDarkMode"
                        >
                          <fa :icon="['fas', 'adjust']" class="mr4 ml4" />
                          Toggle {{ dark ? 'light' : 'dark' }} mode
                        </el-button>
                      </li>
                      <li class="w-fit-full h10" @click="logOutUser">
                        <el-button
                          type="text"
                          class="inline-block color-inherit"
                        >
                          <fa :icon="['fas', 'sign-out-alt']" class="mr5 ml4" />
                          <strong>Sign out</strong>
                        </el-button>
                      </li>
                    </ul>
                    <div
                      slot="reference"
                      class="no-outline circle icon-wrapper"
                      tabindex="0"
                      @click="handleUserButtonDynamics"
                      @keyup.enter.space="handleUserButtonDynamics"
                    >
                      <el-avatar
                        v-if="userImage"
                        size="small"
                        :src="$auth.user.picture"
                      />
                      <fa
                        v-else
                        :icon="['fas', 'user-circle']"
                        class="md-icon user-icon"
                      />
                    </div>
                  </el-popover>
                </div>
              </li>
            </ul>
          </transition-group>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import sponsors from '../config/navbarSponsors'
import infoMenuLinks from '../config/infoMenuLinks'
import dataCollection from '../mixins/dataCollection'
import MobileDrawer from './MobileDrawer.vue'
import FullScreenSearch from './FullScreenSearch.vue'
import MobileDrawerProfile from './MobileDrawerProfile.vue'
import { TOGGLE_DARK } from '../store/actionTypes'

export default {
  name: 'INavbar',
  components: {
    IFilter: () => import('./Filter'),
    ISearch: () => import('./Search'),
    BottomSheet: () => import('./BottomSheet'),
    PremiumPartnersButton: () => import('./PremiumPartners'),
    IMobileDrawer: MobileDrawer,
    IFullScreenSearch: FullScreenSearch,
    IMobileDrawerProfile: MobileDrawerProfile
  },
  mixins: [dataCollection],
  props: {
    isUserNavbar: {
      type: Boolean,
      default: () => false
    }
  },
  data: () => ({
    sponsors,
    search: '',
    infoMenuLinks,
    searchResults: [],
    isDrawerOpen: false,
    isUserMenuOpen: false,
    isInfoMenuOpen: false,
    isIxpsMenuOpen: false,
    isNetworsMenuOpen: false,
    isSponsorsMenuOpen: false,
    isPartnersMenuOpen: false,
    isSubmarineMenuOpen: false,
    isDataCentersMenuOpen: false
  }),
  computed: {
    dark() {
      return this.$store.state.isDark
    },
    userImage() {
      return this.$auth.user && this.$auth.user.picture
    },
    popoverClassGiver() {
      let c = 'popover'
      if (this.dark) c += ' dark'
      else c += ' light'
      return c
    },
    imageURL() {
      return this.dark
        ? 'https://cdn.infrapedia.com/logos/dark-mode-logo.svg'
        : 'https://cdn.infrapedia.com/logos/light-mode-logo.svg'
    },
    isProfileRoute() {
      let isProfile = false
      if (this.$route.name === 'user' || this.$route.name === 'user-profile') {
        isProfile = true
      }
      return isProfile
    },
    userName() {
      return this.$auth.user.name
    }
  },
  async mounted() {
    await this.loadPremiumPartners()
  },
  methods: {
    toggleDarkMode() {
      return this.$store.commit(`${TOGGLE_DARK}`, !this.dark)
    },
    toggleDrawerVisibility() {
      this.isDrawerOpen = !this.isDrawerOpen
    },
    async handleBeforeLoadItem(item) {
      await this.handleItemListSelection(item)
    },
    closeUnwantedOpenMenus() {
      if (this.isInfoMenuOpen) this.isInfoMenuOpen = false
    },
    toggleInfoMenuVisibility() {
      this.isInfoMenuOpen = !this.isInfoMenuOpen
    },
    toggleUserMenuVisibility() {
      this.isUserMenuOpen = !this.isUserMenuOpen
    },
    logOutUser() {
      window.localStorage.removeItem('auth.token-session')
      return this.$auth.logout()
    },
    handleUserButtonDynamics() {
      const { isAuthenticated } = this.$auth

      if (this.$auth && !window.localStorage.getItem('auth.token-session')) {
        this.$router.push('/login')
      } else if (this.$route.name.includes('user')) {
        this.toggleUserMenuVisibility()
      } else if (isAuthenticated) {
        this.$router.push('/user')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/scss/components/navbar-styles.scss';
</style>
