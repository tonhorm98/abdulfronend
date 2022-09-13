<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <h1 class="ma-2 pa-2" style="display:inline;">
            โปรโมชั่น
          </h1>
          <h2 style="display:inline;" class="font-weight-light grey--text text--darken-2">
            PROMOTION
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
              <add-new-promotion @addPromotion="addData($event)" />
            </v-col>
            <!--text field for view and edit data here  ต่อไปอาจจะรวม add data ข้างบนมาด้วย-->
            <v-dialog
              v-model="dialog"
              width="1000"
              height="2000"
              persistent
            >
              <v-card>
                <v-card-title>
                  <span class="text-h5">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="5">
                        picture
                      </v-col>
                      <v-col cols="7">
                        <v-row><h4>เลือกบริษัท</h4></v-row>
                        <v-row>
                          <v-autocomplete
                            v-model="editedItem.companyName"
                            :items="companyList"
                            item-text="name"
                            outlined
                            dense
                            no-data-text="ไม่พบร้าน"
                            placeholder="Amazon"
                          />
                        </v-row>
                        <v-row><h4>หัวข้อเรื่องโปรโมชั่น</h4></v-row>
                        <v-row>
                          <v-text-field
                            v-model="editedItem.topic"
                            required
                            outlined
                            dense
                            placeholder="เมนูต้องโดน"
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
                            placeholder="โปรโมชั่น ส่วนลด ที่บ่อยครั้งมักจะควบคู่ไปกับบัตรเติมน้ำมันของปั๊มปตทเพื่อสะสมแต้ม สำหรับผู้ที่เดินทางบ่อยหรือขับรถเข้าปั๊มปตท.เป็นประจำก็จะสามารถนำแต้มที่สะสมไว้แล้วตามเงื่อนไขไปใช้ได้ด้วยเช่นกัน นอกจากนี้ร้านคาเฟ่นี้ก็ยังมีโปรโมชั่นหรือส่วนลดที่เข้าร่วมกับเครือข่ายโทรศัพท์มือถือ"
                          />
                        </v-row>
                        <v-row><h4>วันที่เริ่ม-สิ้นสุด</h4></v-row>
                        <v-row>
                          <!-- problem here.............................................................................................................. -->
                          <v-dialog
                            ref="dialog"
                            v-model="modal"
                            :return-value.sync="dates"
                            persistent
                            width="290px"
                          >
                            <template #activator="{ on, attrs }">
                              <v-text-field
                                v-model="editedItem.date"
                                placeholder="1 ส.ค. 2565 - 18 ส.ค. 2565"
                                append-icon="mdi-calendar-blank-outline"
                                readonly
                                v-bind="attrs"
                                outlined
                                v-on="on"
                              />
                            </template>
                            <v-date-picker
                              v-model="dates"
                              range
                              locale="th-th"
                            >
                              <v-spacer />
                              <v-btn
                                text
                                color="primary"
                                @click="modal = false"
                              >
                                Cancel
                              </v-btn>
                              <!-- ถ้าไม่มี showDate when chick ok จะไม่มี data show เพราะ data ในช่องไปผูกกับ dates (in v-data-picker)-->
                              <v-btn
                                text
                                color="primary"
                                @click="$refs.dialog.save(dates); showDate"
                              >
                                OK
                              </v-btn>
                            </v-date-picker>
                          </v-dialog>
                        </v-row>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer />
                  <div v-if="isView === false">
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="close"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      color="blue darken-1"
                      text
                      @click="save"
                    >
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
              </v-card>
            </v-dialog>
            <v-col
              cols="6"
              align="right"
            >
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
                v-model="selected"
                show-select
                item-key="no"
                :headers="headers"
                :items="promotions"
                sort-by="no"
                class="elevation-1"
              >
                <template #[`item.picture`]="{ item }">
                  <tr>
                    <td><img :src="item.picture" width="50" height="50"></td>
                  </tr>
                </template>
                <template #[`item.actions`]="{ item }">
                  <v-menu offset-y>
                    <template #activator="{ on, attrs }">
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
                        v-for="(list, index) in menu"
                        :key="index"
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
                </template>
                <template #no-data>
                  <v-btn
                    color="primary"
                    @click="initialize"
                  >
                    Reset
                  </v-btn>
                </template>
              </v-data-table>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
    <v-row>{{ selected }}</v-row>
  </v-container>
</template>

<script>
export default {
  name: 'NewPromotionPage',
  data: () => ({
    selected: [],
    isDisable: false,
    isView: false,
    companyList: ['Amazon', 'Starbuck', 'KAMU'], // hardcode, show get from db
    menu: [
      { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
      { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }],
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'No',
        align: 'start',
        sortable: false,
        value: 'no'
      },
      { text: 'รูปภาพ', value: 'picture' },
      { text: 'หัวข้อเรื่องโปรโมชั่น', value: 'topic' },
      { text: 'วันที่เริ่มต้น-สิ้นสุด', value: 'date' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    promotions: [],
    editedIndex: -1,
    editedItem: {
      no: '',
      companyName: '',
      picture: '',
      topic: '',
      date: '',
      dates: []
      // add startDate
      // add endDate
    },
    defaultItem: {
      no: '',
      companyName: '',
      picture: '',
      topic: '',
      date: '',
      dates: []
    },
    modal: false,
    dates: [],
    date: '7 พ.ค. 2565 - 19 พ.ค. 2565'
  }),

  computed: {
    formTitle () {
      if (this.isView === true) { return 'ดูรายละเอียด' }
      return this.editedIndex === -1 ? 'Add New Promotion เพิ่มโปรโมชั่น' : 'แก้ไขโปรโมชั่น'
    },
    showDate () {
      if (this.dates.length === 0) {
        return this.dates
      }
      const newdate = this.dates
      newdate.sort()
      const start = this.dates[0]
      const options = {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
      }

      const startDay = start.substring(8)
      const startMonth = start.substring(5, 7) - 1
      const startYear = start.substring(0, 4)
      const eventStart = new Date(startYear, startMonth, startDay)
      const startDate = eventStart.toLocaleString('th-TH', options)

      let endDate = ''
      let sep = ''
      if (this.dates.length > 1) {
        const end = this.dates[1]
        const endDay = end.substring(8)
        const endMonth = end.substring(5, 7) - 1
        const endYear = end.substring(0, 4)
        const eventEnd = new Date(endYear, endMonth, endDay)
        endDate = eventEnd.toLocaleString('th-TH', options)
        sep = '-'
      }
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.date = `${startDate} ${sep} ${endDate}`
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.editedItem.date = `${startDate} ${sep} ${endDate}`
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.dates = []

      return this.date
    }
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    }
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.promotions = [
        {
          no: 1,
          companyName: 'Amazon',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เมนูต้องโดน',
          detail: 'detail detail detail 1',
          date: '17 พ.ค. 2565 - 10 พ.ค. 2565'
        },
        {
          no: 2,
          companyName: 'Starbucks',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 1 แถม 1',
          detail: 'detail detail detail 2',
          date: '1 พ.ค. 2565 - 15 พ.ค. 2565'
        },
        {
          no: 3,
          companyName: 'Starbucks',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 2 แถม 1',
          detail: 'detail detail detail 3',
          date: '7 พ.ค. 2565 - 19 พ.ค. 2565'
        },
        {
          no: 4,
          companyName: 'Amazon',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เมนูต้องโดน',
          detail: 'detail detail detail 4',
          date: '17 พ.ค. 2565 - 10 พ.ค. 2565'
        },
        {
          no: 5,
          companyName: 'Starbucks',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 1 แถม 1',
          detail: 'detail detail detail 5',
          date: '1 พ.ค. 2565 - 15 พ.ค. 2565'
        },
        {
          no: 6,
          companyName: 'Kamu',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 2 แถม 1',
          detail: 'detail detail detail 6',
          date: '7 พ.ค. 2565 - 19 พ.ค. 2565'
        }
      ]
    },

    // not use
    editItem (item) {
      this.editedIndex = this.promotions.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.promotions.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      this.promotions.splice(this.editedIndex, 1)
      this.closeDelete()
    },
    // above not use

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
      this.dates = []
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.promotions[this.editedIndex], this.editedItem)
      } else {
        this.promotions.push(this.editedItem)
      }
      this.dates = []
      this.close()
    },

    menuActionClick (action, item) {
      if (action === 'view') {
        this.isView = true
        this.isDisable = true
        this.editedIndex = this.promotions.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      } else if (action === 'edit') {
        this.isView = false
        this.isDisable = false
        this.editedIndex = this.promotions.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      }
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
          const result = this.promotions.filter(item => no.indexOf(item.no) === -1)
          this.promotions = [...result]
          this.selected = []
          this.$swal.fire(
            'Deleted!',
            'Your selected items have been deleted.',
            'success'
          )
        }
      })
    },
    addData (promotionDetail) {
      const newPromotion = {}
      newPromotion.topic = promotionDetail.topic
      newPromotion.date = promotionDetail.date
      newPromotion.companyName = promotionDetail.companyName
      newPromotion.picture = 'https://cdn.vuetifyjs.com/images/john.jpg' // hardcode
      newPromotion.no = this.promotions[this.promotions.length - 1].no + 1
      newPromotion.action = [
        { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
      this.promotions.push(newPromotion)
    }
  }
}
</script>

<style>

</style>
