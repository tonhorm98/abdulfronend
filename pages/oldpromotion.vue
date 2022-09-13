<template>
  <v-container>
    <v-dialog v-model="showDialog" width="1000" height="2000" persistent>
      <v-card>
        <v-card-title class="blue darken-4 white--text">
          edit/view
        </v-card-title>
        <v-form ref="inputForm" class="px-4 py-6 h-100 mx-5" @submit.prevent="submitForm">
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
                  placeholder="คาเฟ่ อเมซอน"
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
                <v-dialog
                  ref="dialog"
                  v-model="modal"
                  :return-value.sync="dates"
                  persistent
                  width="290px"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="showDate"
                      placeholder="1 ส.ค. 2565 - 18 ส.ค. 2565"
                      append-icon="mdi-calendar-blank-outline"
                      readonly
                      v-bind="attrs"
                      outlined
                      v-on="on"
                    />
                  </template>
                  <v-date-picker
                    v-model="editedItem.dates"
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
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.dialog.save(dates)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-dialog>
              </v-row>
            </v-col>
          </v-row>
          <v-row>
            <v-spacer />
            <v-btn
              color="grey lighten-1"
              class="white--text mb-1 me-4"
              @click="showDialog = false"
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
              type="submit"
            >
              <v-icon
                left
              >
                mdi-content-save-outline
              </v-icon>
              Save
            </v-btn>
          </v-row>
        </v-form>
      </v-card>
    </v-dialog>
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

            <v-col
              cols="6"
              align="right"
            >
              <delete-no :selected="selected" @deleteByNo="deleteData($event)" />
            </v-col>
          </v-row>

          <v-row>
            <v-col
              cols="12"
            >
              <v-data-table
                id="headtable"
                v-model="selected"
                :headers="headers"
                :items="promotions"
                item-key="no"
                :items-per-page="10"
                class="elevation-1 mx-3"
                group-by="companyName"
                show-select
              >
                <template #[`item.picture`]="{ item }">
                  <tr>
                    <td><img :src="item.picture" width="50" height="50"></td>
                  </tr>
                </template>
                <template #[`item.action`]="{ item }">
                  <tr>
                    <td>
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
                            v-for="(list, index) in item.action"
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
  name: 'PromotionPage',
  data () {
    return {
      showDialog: false,
      modal: false,
      selected: [],
      headers: [
        {
          text: 'No.',
          align: 'start',
          sortable: false,
          value: 'no'
        },
        { text: 'รูปภาพ', value: 'picture' },
        { text: 'หัวข้อเรื่องโปรโมชั่น', value: 'topic' },
        { text: 'วันที่เริ่มต้น-สิ้นสุด', value: 'date' },
        { text: 'Action', value: 'action' }
      ],
      promotions: [
        {
          no: 1,
          companyName: 'Amazon',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เมนูต้องโดน',
          detail: 'detail detail detail 1',
          date: '17 พ.ค. 2565 - 10 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
        },
        {
          no: 2,
          companyName: 'Starbucks',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 1 แถม 1',
          detail: 'detail detail detail 2',
          date: '1 พ.ค. 2565 - 15 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
        },
        {
          no: 3,
          companyName: 'Kamu',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 2 แถม 1',
          detail: 'detail detail detail 3',
          date: '7 พ.ค. 2565 - 19 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
        },
        {
          no: 4,
          companyName: 'Amazon',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เมนูต้องโดน',
          detail: 'detail detail detail 4',
          date: '17 พ.ค. 2565 - 10 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
        },
        {
          no: 5,
          companyName: 'Starbucks',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 1 แถม 1',
          detail: 'detail detail detail 5',
          date: '1 พ.ค. 2565 - 15 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
        },
        {
          no: 6,
          companyName: 'Kamu',
          picture: 'https://cdn.vuetifyjs.com/images/john.jpg',
          topic: 'เครื่องดื่มทุกเมนู ซื้อ 2 แถม 1',
          detail: 'detail detail detail 6',
          date: '7 พ.ค. 2565 - 19 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
        }
      ],
      editedIndex: -1,
      editedItem: {
        companyName: '',
        picture: '',
        topic: '',
        detail: '',
        date: '',
        dates: []
      },
      defaultItem: {
        companyName: '',
        picture: '',
        topic: '',
        detail: '',
        date: '',
        dates: []
      },
      dates: []

    }
  },
  computed: {
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
      return this.date
    }
  },
  methods: {
    menuActionClick (action, item) {
      // console.log(action)
      // console.log(item.no)
      if (action === 'view') {
        alert('view')
      } else if (action === 'edit') {
        this.editedIndex = this.promotions.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.showDialog = true
      }
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
    },
    deleteData (no) {
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
          // eslint-disable-next-line unicorn/prefer-includes
          const result = this.promotions.filter(item => no.indexOf(item.no) === -1)
          this.promotions = [...result]
          this.$swal.fire(
            'Deleted!',
            'Your selected items have been deleted.',
            'success'
          )
        }
      })
    }
  }
}

</script>

<style>
.style-1 {
  background-color: rgb(215,215,44)
}
.style-2 {
  background-color: rgb(114,114,67)
}

.v-data-table-header{
  background-color: #273A68;
}

#headtable table thead tr th{
  color: white;
}
/* .v-input--selection-controls__ripple{
  color: white;
} */
</style>
