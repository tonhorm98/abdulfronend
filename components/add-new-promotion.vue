<template>
  <div>
    <v-btn
      color="blue darken-4 white--text"
      class="mx-3"
      @click="showDialog = true"
    >
      <v-icon left>
        mdi-plus-circle-outline
      </v-icon>
      Add New
    </v-btn>
    <v-dialog v-model="showDialog" width="1000" height="2000" persistent>
      <v-card>
        <v-card-title class="blue darken-4 white--text">
          Add New Promotion เพิ่มโปรโมชั่น
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
                  v-model="nameEN"
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
                  v-model="topic"
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
                  v-model="detail"
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
                      v-model="date"
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
  </div>
</template>

<script>
export default {
  data () {
    return {
      showDialog: false,
      companyList: [], // hardcode, show get from db
      nameEN: '',
      topic: '',
      detail: '',
      startDate: '',
      endDate: '',
      modal: false,
      dates: [],
      date: ''
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
      const startDateLocale = eventStart.toLocaleString('th-TH', options)

      console.log(startDay)
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.startDate = this.dates[0]

      const end = this.dates[1]
      const endDay = end.substring(8)
      const endMonth = end.substring(5, 7) - 1
      const endYear = end.substring(0, 4)
      const eventEnd = new Date(endYear, endMonth, endDay)
      const endDateLocale = eventEnd.toLocaleString('th-TH', options)
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.endDate = this.dates[1]

      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.date = `${startDateLocale} - ${endDateLocale}`
      return this.date
    }
  },
  mounted () {
    this.initialize()
  },
  methods: {
    submitForm () {
      const promotionDetail = {}
      promotionDetail.topic = this.topic
      promotionDetail.date = this.date
      promotionDetail.nameEN = this.nameEN
      promotionDetail.detail = this.detail
      promotionDetail.startDate = this.startDate
      promotionDetail.endDate = this.endDate

      this.$emit('addPromotion', promotionDetail)
      this.$swal.fire({
        icon: 'success',
        title: 'Your work has been saved',
        showConfirmButton: true,
        timer: 2000
      })
      this.$refs.inputForm.reset()
      this.showDialog = false
    },
    async initialize () {
      const getcompanies = await this.$axios.$get('http://localhost:8972/company')
      for (let i = 0; i < getcompanies.length; i += 1) {
        this.companyList.push(getcompanies[i].nameEN)
      }
    }
  }

}
</script>

<style>
.require{
    color: red;
}
</style>
