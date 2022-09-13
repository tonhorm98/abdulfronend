<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <h1 class="ma-2 pa-2" style="display:inline;">
            ข้อมูลผู้ใช้
          </h1>
          <h2 style="display:inline;" class="font-weight-light grey--text text--darken-2">
            USER
          </h2>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <v-row>
            <v-col
              cols="6"
            >
              <!-- change to add-new-user -->
              <add-new-user @addUser="addUser($event)" />
            </v-col>
            <!-- page user ไม่มีปุ่ม delete -->
          </v-row>

          <v-row>
            <v-col
              cols="12"
            >
              <v-data-table
                id="headtable"
                :headers="headers"
                :items="users"
                item-key="no"
                class="elevation-1 mx-3"
              >
                <!-- ประมาณว่าสร้าง el แล้วใส่ลงใน column action-->
                <template #[`item.action`]="{ item }">
                  <tr>
                    <td>
                      <v-menu offset-y>
                        <template #activator="{ on, attrs }">
                          <!-- ยังไม่เข้าใจเท่าไหร่ในเรื่องของ on attrs-->
                          <v-btn
                            depressed
                            v-bind="attrs"
                            v-on="on"
                          >
                            Action
                            <v-icon
                              right
                            >
                              mdi-chevron-down
                            </v-icon>
                          </v-btn>
                        </template>
                        <v-list>
                          <v-list-item
                            v-for="(list, index) in item.action"
                            :key="index"
                            link
                          >
                            <v-list-item-title>
                              <v-icon
                                left
                                :color="list.color"
                              >
                                {{ list.icon }}
                              </v-icon>
                              {{ list.title }}
                            </v-list-item-title>
                          </v-list-item>
                        </v-list>
                      </v-menu>
                    </td>
                  </tr>
                </template>
              </v-data-table>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
//  blue darken-4 white--text
export default {
  name: 'UserPage',
  data () {
    return {
      singleSelect: false,
      headers: [ // ประมาณ column แรก
        {
          text: 'No.', // ชื่อ column แรก
          align: 'start',
          sortable: false,
          value: 'no' // ชื่อตัวแปรที่เอามาใส่ใน column แรก
        },
        { text: 'ชื่อ-นามสกุล', value: 'fullName' },
        { text: 'อีเมล์', value: 'email' },
        { text: 'เบอร์โทรศัพท์', value: 'tel' },
        { text: 'รหัสผ่าน', value: 'passwd' },
        { text: 'Action', value: 'action' }
      ],
      users: [
        {
          no: 1,
          fullName: 'Elon Musk',
          email: 'elon@gmail.com',
          tel: '0900000000',
          passwd: '12345678',
          action: [
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' },
            { title: 'Delete', icon: 'mdi-delete', color: 'red' }]

        },
        {
          no: 2,
          fullName: 'Bill Gate',
          email: 'bill@yahoo.com',
          tel: '0812345678',
          passwd: '12341234',
          action: [
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' },
            { title: 'Delete', icon: 'mdi-delete', color: 'red' }]
        },
        {
          no: 3,
          fullName: 'Mark Z',
          email: 'mark@fb.com',
          tel: '0801231234',
          passwd: '11112222',
          action: [
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' },
            { title: 'Delete', icon: 'mdi-delete', color: 'red' }]
        }
      ]
    }
  },
  methods: {
    addUser (UserDetail) {
      const newUser = {}
      newUser.fullName = UserDetail.fullName
      newUser.email = UserDetail.email
      newUser.tel = UserDetail.tel
      newUser.passwd = UserDetail.passwd
      newUser.no = this.users[this.users.length - 1].no + 1
      newUser.action = [
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange' },
        { title: 'Delete', icon: 'mdi-delete', color: 'red' }]
      this.users.push(newUser)
    }
  }
}
</script>

<style>
.v-data-table-header{
  background-color: #273A68;
}
#headtable table thead tr th{
  color: white;
}
</style>
