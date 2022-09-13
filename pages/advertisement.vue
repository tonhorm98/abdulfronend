<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <!-- เดี๋ยวอาจปรับอีก  -->
          <h1 class="ma-2 pa-2" style="display:inline;">
            ประชาสัมพันธ์
          </h1>
          <h2 style="display:inline;" class="font-weight-light grey--text text--darken-2">
            ADVERTISEMENT
          </h2>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <!-- ปุ่ม add delete -->
          <v-row>
            <v-col
              cols="6"
            >
              <add-new-advertisement @addAdvertisement="addData($event)" />
              <!-- edit here for view and edit maybe create later -->
              <v-dialog
                v-model="showDialog"
                width="1000"
                persistent
              >
                <v-card>
                  <v-card-title class="blue darken-4 white--text">
                    <span class="text-h5">{{ formTitle }}</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="5">
                          picture
                        </v-col>
                        <v-col cols="7">
                          <v-row><h4>หัวข้อเรื่องประชาสัมพันธ์</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.topic"
                              required
                              outlined
                              dense
                              :disabled="isDisable"
                              placeholder="ติดต่อจองบูธและสอบถามทั่วไป"
                            />
                          </v-row>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col cols="5" />
                        <v-col cols="7">
                          <v-row><h4>รายละเอียด</h4></v-row>
                          <v-row>
                            <v-textarea
                              v-model="editedItem.detail"
                              outlined
                              auto-grow
                              rows="7"
                              row-height="30"
                              dense
                              :disabled="isDisable"
                              placeholder="GPF Property Management Limited บริษัทจีพีเอฟ พร็อพเพอร์ตี้แมเนจเม้นท์ จำกัด - 990 อาคารอับดุลราฮิม ชั้น 21 ถนนพระราม 4 แขวงสีลม เขตบางรัก กรุงเทพ 10500- Tel. 02-636-1990- Email : mkt@gpfpro.co.th"
                            />
                          </v-row>
                        </v-col>
                      </v-row>
                      <v-card-actions>
                        <v-spacer />
                        <div v-if="isView === false">
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
                        </div>
                        <div v-else>
                          <v-btn
                            color="blue darken-1"
                            text
                            @click="close"
                          >
                            Close
                          </v-btn>
                        </div>
                      </v-card-actions>
                    </v-container>
                  </v-card-text>
                </v-card>
              </v-dialog>
            </v-col>
            <v-col
              cols="6"
              align="right"
            >
              <!-- <delete-no :selected="selected" @deleteByNo="deleteData($event)" /> -->
              <v-btn
                color="error"
                class="mx-3"
                @click="deleteByNo (selected)"
              >
                <v-icon left>
                  mdi-delete
                </v-icon>
                Delete
              </v-btn>
            </v-col>
          </v-row>

          <v-row>
            <v-col
              cols="12"
            >
              <v-data-table
                id="headtable"
                v-model="selected"
                show-select
                :headers="headers"
                :items="advertisements"
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
                <!-- ประมาณว่าสร้าง el รูปภาพ แล้วใส่ลงใน value picStore-->
                <!-- ประมาณว่าแปลงเป็น el รูปภาพก่อน แล้วค่อยเอาไปแทนในค่าที่ value:'picStore'-->

                <template #[`item.picture`]="{ item }">
                  <tr>
                    <td>
                      <img :src="item.picture" width="60" height="60">
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
  name: 'NewAdvertisePage',
  data () {
    return {
      showDialog: false,
      isDisable: false,
      isView: false,
      selected: [],
      editedIndex: -1, // ยังงงๆอยู่ ไม่มีก็ได้มั้ง
      headers: [ // ประมาณ column แรก
        {
          text: 'No.', // ชื่อ column แรก
          align: 'start',
          sortable: false,
          value: 'no' // ชื่อตัวแปรที่เอามาใส่ใน column แรก
        },
        { text: 'รูปภาพ', value: 'picture' },
        { text: 'หัวข้อเรื่องประชาสัมพันธ์', value: 'topic' },
        { text: 'Action', value: 'action' }
      ],
      advertisements: [
        {
          no: 1,
          storeName: '',
          picture: 'https://images.unsplash.com/photo-1481833761820-0509d3217039?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80',
          topic: 'ติดต่อจองบูธ',
          detail: 'testtest1'
        },
        {
          no: 2,
          storeName: '',
          picture: 'https://images.unsplash.com/photo-1481833761820-0509d3217039?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80',
          topic: 'ข้อมูลการเช่าสำนักงานอับดุลราฮิมเพลส',
          detail: 'testtest2'
        },
        {
          no: 3,
          storeName: '',
          picture: 'https://images.unsplash.com/photo-1481833761820-0509d3217039?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80',
          topic: 'ติดต่อพื้นที่เช่าสำนักงานและร้านค้า',
          detail: 'testtest3'
        }
      ],
      actions: [
        { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }
      ],
      editedItem: {
        no: '',
        picture: '',
        topic: '',
        detail: ''
      }
    }
  },
  computed: {
    formTitle () {
      if (this.isView === true) { return 'ดูรายละเอียด' }
      //   return this.editedIndex === -1 ? 'Add New Promotion เพิ่มโปรโมชั่น' : 'แก้ไขโปรโมชั่น'
      return 'แก้ไขข้อมูลประชาสัมพันธ์'
    }
  },
  methods: {
    addData (advertisementDetail) {
      const newAdvertisement = {}
      newAdvertisement.storeName = advertisementDetail.storeName
      newAdvertisement.topic = advertisementDetail.topic
      newAdvertisement.detail = advertisementDetail.detail
      newAdvertisement.no = this.advertisements[this.advertisements.length - 1].no + 1
      newAdvertisement.action = [
        { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
      this.advertisements.push(newAdvertisement)
    },
    deleteByNo (selected) {
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
          const no = []
          for (let i = 0; i < selected.length; i += 1) {
            no.push(selected[i].no)
          }
          // eslint-disable-next-line unicorn/prefer-includes
          const result = this.advertisements.filter(item => no.indexOf(item.no) === -1)
          this.advertisements = [...result]
          this.selected = []
          this.$swal.fire(
            'Deleted!',
            'Your selected items have been deleted.',
            'success'
          )
        }
      })
    },
    menuActionClick (action, item) {
      if (action === 'view') {
        this.isView = true
        this.isDisable = true
        this.editedIndex = this.advertisements.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.showDialog = true
      } else if (action === 'edit') {
        this.isView = false
        this.isDisable = false
        this.editedIndex = this.advertisements.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.showDialog = true
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
        Object.assign(this.advertisements[this.editedIndex], this.editedItem)
      } else {
        // case when create data
        this.advertisements.push(this.editedItem)
      }
      this.close()
    }
  }
}
</script>

<style>
.v-data-table-header{
  background-color: #0D47A1;
}
#headtable table thead tr th{
  color: white;
}
img{
    margin-top: 6px;
}
#container {
    display: flex;
    justify-content: space-between;
}
</style>
