<template>
  <div class="h-screen">
    <header-top />

    <div class="drawer drawer-mobile menu-nav">
      <input
        id="main-menu"
        type="checkbox"
        class="drawer-toggle"
        v-model="showMainMenu"
      />
      <main
        class="
          flex-grow
          block
          w-full
          pt-16
          overflow-x-hidden
          bg-base-100
          text-base-content
          drawer-content
        "
      >
        <div class="p-4 lg:p-10 min-h-full">
          <Nuxt />
        </div>
      </main>
      <div class="drawer-side h-full">
        <label for="main-menu" class="drawer-overlay"></label>
        <aside
          class="
            flex flex-col
            justify-between
            pt-16
            border-r border-base-200
            bg-base-100
            text-base-content
            w-60
          "
        >
          <div>
            <Menu class="flex flex-col p-4 pb-10 compact">
              <MenuItem v-for="(item, i) in menuItems" :key="i" class="mb-3">
                <NuxtLink
                  v-on:click.native="showMainMenu = false"
                  class="capitalize "
                  :to="item.link"
                >
                  <fa :icon="item.icon" class="mr-2" />
                  {{ item.title }}
                </NuxtLink>
              </MenuItem>
            </Menu>
          </div>
        </aside>
      </div>
    </div>
    <footer-bar />
  </div>
</template>

<script>
import { themeChange } from "theme-change";
import HeaderTop from "../components/Navbar/HeaderTop.vue";
import FooterBar from "../components/footer/footerBar.vue";
export default {
  components: { HeaderTop, FooterBar },
  props: {
    classes: String,
  },
  data() {
    return {
      docPages: [],
      corePages: [],
      componentPages: [],
      demoPages: [],
      showMainMenu: false,
    };
  },
  created() {
    this.$router.options.routes.forEach((routeOption) => {
      if (routeOption.path.startsWith("/docs")) {
        this.docPages.push({
          name: routeOption.name.replace("docs-", ""),
          path: routeOption.path,
        });
      }
    });
    this.$router.options.routes.forEach((routeOption) => {
      if (routeOption.path.startsWith("/core")) {
        this.corePages.push({
          name: routeOption.name.replace("core-", ""),
          path: routeOption.path,
        });
      }
    });
    this.$router.options.routes.forEach((routeOption) => {
      if (routeOption.path.startsWith("/components")) {
        this.componentPages.push({
          name: routeOption.name.replace("components-", ""),
          path: routeOption.path,
        });
      }
    });
    this.$router.options.routes.forEach((routeOption) => {
      if (routeOption.path.startsWith("/demos")) {
        this.demoPages.push({
          name: routeOption.name.replace("demos-", ""),
          path: routeOption.path,
        });
      }
    });
  },
  computed: {
    menuItems() {
      return this.$store.state.menu;
    },
  },
  mounted() {
    themeChange(false);
  },
};
</script>

<style scoped>
.menu-nav {
  height: calc(100% - 56px);
}
</style>
