<template>
  <v-app dark>
    <!-- Other components remain unchanged -->
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      style="background-color: #73AB6B;"
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
          class="white-text"
        >
          <v-list-item-action>
            <v-icon class="white-icon">{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title class="white-text">{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:prepend>
        <v-list-item two-line>
          <v-list-item-avatar>
            <img src="../static/profile.jpg">
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title class="white-text">{{ $auth.user.first_name }} {{ $auth.user.middle_initial }} {{ $auth.user.last_name }}</v-list-item-title>
            <v-list-item-subtitle class="white-text">Logged In</v-list-item-subtitle>
            <v-list-item-subtitle class="white-text">Admin</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </template>

      <template v-slot:append>
        <div class="pa-2">
          <v-btn block @click="$auth.logout()" style="font-weight: bold;">
            Logout
          </v-btn>
        </div>
      </template>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      style="background-color: #73AB6B;"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" class="white-icon" />
      <v-btn
        icon
        @click.stop="clipped = !clipped"
        class="white-icon"
      >
        <v-icon>mdi-account-group</v-icon>
      </v-btn>
      <v-toolbar-title class="white-text">{{ title }}</v-toolbar-title>
      <v-spacer />
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
        class="white-icon"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Nuxt />
    </v-main>

    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon class="white-icon">
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title class="white-text">Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  middleware: 'auth',
  data() {
    return {
      selectedItem: 0,
      clipped: false,
      drawer: true,
      fixed: false,
      items: [
        {
          icon: "mdi-view-dashboard",
          title: "Dashboard",
          to: "/",
        },
        {
          icon: "mdi-account-box",
          title: "Account List",
          to: "/account",
        },
        {
          icon: "mdi-account-tie",
          title: "Employee List",
          to: "/employeelist",
        },
        {
          icon: "mdi-cog",
          title: "Settings",
          to: "/settings",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "EMPLOYEE MANAGEMENT SYSTEM",
    };
  },
};
</script>


<style>


.white-text {
  color: white !important;
}

.white-icon {
  color: white !important;
}


</style>