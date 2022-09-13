<template>
  <v-app dark>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-city</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-avatar
        size="50"
      >
        <img
          src="https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=764&q=80"
        >
      </v-avatar>
    </v-app-bar>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-container>
        <h3>รายการทั่วไป</h3>
        <v-list>
          <v-list-item
            v-for="(item, i) in generals"
            :key="i"
            :to="item.to"
            router
            exact
            active-class="blue darken-4 white--text"
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
        </v-list>
        <h3>ตั้งค่าทั่วไป</h3>
        <v-list>
          <v-list-item
            v-for="(item, i) in settings"
            :key="i"
            :to="item.to"
            router
            exact
            active-class="blue darken-4 white--text"
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-container>
    </v-navigation-drawer>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
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
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: true,
      drawer: false,
      fixed: false,
      generals: [
        {
          title: 'บริษัทคู่ค้า',
          icon: 'mdi-store',
          to: '/company'
        },
        {
          icon: 'mdi-layers-triple-outline ',
          title: 'ประชาสัมพันธ์',
          to: '/advertisement'
        },
        {
          icon: 'mdi-bullhorn',
          title: 'โปรโมชั่น',
          to: '/promotion'
        },
        {
          icon: 'mdi-purse',
          title: 'การชำระเงิน',
          to: '/payment'
        }
      ],
      settings: [
        {
          icon: 'mdi-account',
          title: 'ข้อมูลผู้ใช้',
          to: '/user'
        },
        {
          icon: 'mdi-account-group',
          title: 'กลุ่มข้อมูลผู้ใช้',
          to: '/usergroup'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'ABDULRAHIM PLACE'
    }
  }
}
</script>
