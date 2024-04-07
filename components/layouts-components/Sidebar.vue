<template>
  <div>
    <div class="sidebar text-center">
      <img
        class="brand mt-3"
        src="@/assets/img/tfgmpclogo.jpg"
        alt="logo Image"
        href="#"
      />
      <div
        v-for="item in menus"
        :key="item.menu"
        :class="item.selected ? 'menu selected' : 'menu '"
        @click="onSelectMenu(item)"
      >
        <font-awesome-icon
          class="menuIcon mb-1"
          :icon="['fas', item.icon]"
          id="icon"
        />
        <!-- TOOLTIP -->
        <ToolTipLogout v-if="item.selected & (item.menu == 'Profile')" />

        <div class="fsize-10px">
          {{ item.menu }}
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import ToolTipLogout from "@/components/tools/ToolTipLogout.vue";
export default {
  components: { ToolTipLogout },
  data() {
    return {
      menus: [
        { menu: "Admin", icon: "user-tie", selected: false },
        { menu: "Accounting", icon: "calculator", selected: false },
        { menu: "Billing", icon: "file-invoice-dollar", selected: false },
        { menu: "Treasury", icon: "gem", selected: false },
        { menu: "Reports", icon: "chart-line", selected: false },
        { menu: "Profile", icon: "user", selected: false },
      ],
    };
  },
  methods: {
    onSelectMenu(item) {
      this.menus.forEach((val) => {
        val.selected = false;
      });
      item.selected = true;
      localStorage.selectedMenu = item.menu;
    },

    onLogout() {
      this.$router.push("/");
    },
  },

  mounted() {
    this.menus.forEach((val) => {
      if (val.menu == localStorage.selectedMenu) {
        val.selected = true;
      }
    });
  },
};
</script>
<style scoped lang="scss">
.sidebar {
  /* Add a shadow on the right side */
  box-shadow: 5px 0 5px -5px rgba(0, 0, 0, 0.5); /* Adjust shadow color, size, and spread radius as needed */
  height: 100vh;
  width: 7vw;
}

.brand {
  width: 4vw;
  margin-bottom: 6vh;
}

.menu {
  cursor: pointer;
  display: flex;
  flex-direction: column;
  margin: 1vh;
  padding-top: 8px;
  padding-bottom: 8px;

  &Icon {
    font-size: 18px;
    color: #595951;
  }

  &:hover {
    background: whitesmoke;
  }
}

.selected {
  background: whitesmoke;
  border-left: 3px solid skyblue;
}

.sidebar-tooltip {
  left: 8vw;
  position: fixed;
  width: 250px;
  background: skyblue;

  clip-path: polygon(
    6% 0,
    100% 0,
    100% 43%,
    100% 100%,
    68% 100%,
    32% 100%,
    6% 99%,
    6% 18%,
    0 11%
  );
}
</style>
