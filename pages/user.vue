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

              <!-- edit here -->
              <v-dialog
                v-model="showDialog"
                width="600"
                persistent
              >
                <v-card>
                  <v-card-title class="blue darken-4 white--text">
                    <span class="text-h5">{{ formTitle }}</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12">
                          <v-row><h4><b class="require">*</b>ชื่อ-นามสกุล</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.fullName"
                              required
                              outlined
                              dense
                              placeholder="สมชาย สายชม"
                            />
                          </v-row>

                          <v-row><h4><b class="require">*</b>อีเมล์</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.email"
                              required
                              outlined
                              dense
                              placeholder="somchai.s@gmail.com"
                            />
                          </v-row>
                          <v-row><h4><b class="require">*</b>เบอร์โทรศัพท์</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.tel"
                              required
                              outlined
                              dense
                              placeholder="0900000000"
                            />
                          </v-row>
                          <v-row><h4><b class="require">*</b>รหัสผ่าน</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.passwd"
                              required
                              outlined
                              dense
                              placeholder="12345678"
                            />
                          </v-row>
                        </v-col>
                      </v-row>
                      <!-- ปุ่ม cancel & save -->
                      <v-row>
                        <v-spacer />
                        <v-btn
                          color="grey lighten-1"
                          class="white--text mb-1 me-4"
                          @click="close"
                        >
                          <v-icon
                            left
                          >
                            mdi-cancel
                          </v-icon>
                          Close
                        </v-btn>
                        <v-btn
                          class="mb-1"
                          color="success"
                          @click="save"
                        >
                          <v-icon
                            left
                          >
                            mdi-content-save-outline
                          </v-icon>
                          Save
                        </v-btn>
                      </v-row>
                    </v-container>
                  </v-card-text>
                </v-card>
              </v-dialog>
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
                            v-for="(list, index) in actions"
                            :key="index"
                            link
                            @click="menuActionClick(list.doMethod,item)"
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
export default {
  name: 'UserPage',
  data () {
    return {
      showDialog: false,
      isDisable: false,
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
          passwd: '12345678'
        },
        {
          no: 2,
          fullName: 'Bill Gate',
          email: 'bill@yahoo.com',
          tel: '0812345678',
          passwd: '12341234'
        },
        {
          no: 3,
          fullName: 'Mark Z',
          email: 'mark@fb.com',
          tel: '0801231234',
          passwd: '11112222'
        }
      ],
      actions: [
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' },
        { title: 'Delete', icon: 'mdi-delete', color: 'red', doMethod: 'delete' }
      ],
      editedItem: {
        no: '',
        fullname: '',
        email: '',
        tel: '',
        passwd: ''
      }
    }
  },
  computed: {
    formTitle () {
      if (this.isView === true) { return 'ดูรายละเอียด' }
      //   return this.editedIndex === -1 ? 'Add New Promotion เพิ่มโปรโมชั่น' : 'แก้ไขโปรโมชั่น'
      return 'แก้ไขข้อมูลผู้ใช้'
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
    },
    menuActionClick (action, item) {
      if (action === 'view') {
        this.isView = true
        this.isDisable = true
        this.editedIndex = this.users.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.showDialog = true
      } else if (action === 'edit') {
        this.isView = false
        this.isDisable = false
        this.editedIndex = this.users.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.showDialog = true
      } else if (action === 'delete') {
        this.$swal.fire({
          title: 'Are you sure?',
          text: "You won't be able to revert this!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes, delete it!'
        }).then((result) => {
          if (result.isConfirmed) {
            this.editedIndex = this.users.indexOf(item)
            // this.editedItem = Object.assign({}, item)
            this.users.splice(this.editedIndex, 1)
            this.$swal.fire(
              'Deleted!',
              'Your selected items have been deleted.',
              'success'
            )
          }
        })
      }
    },
    close () {
      this.showDialog = false
      this.isDisable = false
      //   this.$nextTick(() => {
      //     this.editedItem = Object.assign({}, this.defaultItem)
      //     this.editedIndex = -1
      //   })
      this.editedIndex = -1
    },
    save () {
      if (this.editedIndex > -1) {
        // case when edit
        Object.assign(this.users[this.editedIndex], this.editedItem)
      } else {
        // case when create data
        this.user.push(this.editedItem)
      }
      this.close()
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
