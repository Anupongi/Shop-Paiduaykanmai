<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" :clipped="clipped" fixed app>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <img src="~/static/logo.png" alt="" />
      <v-toolbar-title v-text="title" class="pl-2" />
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: true,
      fixed: false,
      items: [
        {
          icon: 'mdi-home',
          title: 'ข้อมูลร้านค้าทั้งหมด',
          to: '/',
        },
        {
          icon: 'mdi-basket-fill',
          title: 'ข้อมูลสินค้าทั้งหมด',
          to: '/product',
        },
      ],
      title: 'ระบบจัดการร้านค้าขนมและเครื่องดื่ม',
    }
  },
}
</script>
<style>
.v-application--wrap,
.v-dialog,.v-menu__content {
  font-family: 'Prompt', sans-serif !important;
}
.v-application .text-h5 {
  font-family: 'Prompt', sans-serif !important;
}
.v-data-table {
  width: 100%;
}
th {
  color: #ffffff !important;
  background-color: #1976d2 !important;
}
</style>
