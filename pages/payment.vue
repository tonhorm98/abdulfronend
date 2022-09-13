<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <!-- เดี๋ยวอาจปรับอีก  -->
          <h1 class="ma-2 pa-2" style="display:inline;">
            การชำระเงิน
          </h1>
          <h2 style="display:inline;" class="font-weight-light grey--text text--darken-2">
            PAYMENT
          </h2>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <h4 class="mx-5 pt-3">
            เลือกรายการสถานะ
          </h4>
          <v-row>
            <v-col
              cols="5"
            >
              <v-autocomplete
                :items="itemNumber"
                dense
                solo
                label="รายการใบแจ้งหนี้ยังไม่ชำระ"
                class="mx-4 pt-2"
              />
            </v-col>
          </v-row>

          <v-row>
            <v-col
              cols="6"
            >
              <!-- change to add-new-payment -->
              <!-- <add-new-store @addUser="addUser($event)" /> -->
              <add-new-payment @addStore="addData($event)" />
            </v-col>
            <!-- page shop มีปุ่ม delete -->
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
                show-select
                :headers="headers"
                :items="invoices"
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
                          <!-- ใส่ @click ใน tag <v-list-item> -->
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
                <!-- ประมาณว่าสร้าง el รูปภาพ แล้วใส่ลงใน value picStore-->
                <!-- ประมาณว่าแปลงเป็น el รูปภาพก่อน แล้วค่อยเอาไปแทนในค่าที่ value:'picStore'-->

                <template #[`item.picStore`]="{ item }">
                  <tr>
                    <td>
                      <img :src="item.picStore" width="60" height="60">
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
  name: 'PaymentPage',
  data () {
    return {
      itemnumberlist: [],
      selected: [],
      headers: [ // ประมาณ column แรก
        {
          text: 'No.', // ชื่อ column แรก
          align: 'start',
          sortable: false,
          value: 'no' // ชื่อตัวแปรที่เอามาใส่ใน column แรก
        },
        { text: 'ชื่อผู้เช่า', value: 'tenantName' },
        { text: 'เลขที่ใบแจ้งหนี้', value: 'invoiceNumber' },
        { text: 'วันที่ออกใบแจ้งหนี้', value: 'invoiceStartDate' },
        { text: 'จำนวนเงิน', value: 'price' },
        { text: 'วันที่แจ้ง', value: 'invoiceInformDate' },
        { text: 'วันที่ครบกำหนด', value: 'invoiceDeadlineDate' },
        { text: 'Action', value: 'action' }
      ],
      invoices: [
        {
          no: 1,
          tenantName: 'สมชาย สายชม',
          invoiceNumber: '1112223334445',
          invoiceStartDate: '13 พ.ค. 2565',
          price: '5,000',
          invoiceInformDate: '9 พ.ค. 2565',
          invoiceDeadlineDate: '17 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-close', color: 'red' }]

        },
        {
          no: 2,
          tenantName: 'สมปอง น้องสมชาย',
          invoiceNumber: '1112223334446',
          invoiceStartDate: '13 พ.ค. 2565',
          price: '2,000',
          invoiceInformDate: '9 พ.ค. 2565',
          invoiceDeadlineDate: '17 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-close', color: 'red' }]

        },
        {
          no: 3,
          tenantName: 'สมหมาย สมหมาย',
          invoiceNumber: '1112223334447',
          invoiceStartDate: '13 พ.ค. 2565',
          price: '10,000',
          invoiceInformDate: '9 พ.ค. 2565',
          invoiceDeadlineDate: '17 พ.ค. 2565',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-close', color: 'red' }]

        }
      ]

    }
  },
  computed: {
    itemNumber () {
      for (let i = 0; i < this.invoices.length; i += 1) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.itemnumberlist.push(this.invoices[i].invoiceNumber)
      }
      return this.itemnumberlist
    }
  },
  watch: {
    invoices (value) {
      if (value.length >= 0) {
        this.itemnumberlist = []
        for (let i = 0; i < this.invoices.length; i += 1) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
          this.itemnumberlist.push(this.invoices[i].invoiceNumber)
        }
      }
    }
  },
  methods: {
    // edit later
    addData (storeDetail) {
      const newStore = {}
      newStore.nameStoreTH = storeDetail.nameStoreTH
      newStore.nameStoreEN = storeDetail.nameStoreEN
      newStore.detail = storeDetail.detail
      newStore.no = this.stores[this.stores.length - 1].no + 1
      newStore.action = [
        { title: 'View', icon: 'mdi-eye', color: 'green' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]
      this.stores.push(newStore)
    },
    itemNumberr () { // error not use
      for (let i = 0; i < this.invoices.length; i += 1) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.itemnumberlist.push(this.invoices[i].invoiceNumber)
      }
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
          const result = this.invoices.filter(item => no.indexOf(item.no) === -1)
          this.invoices = [...result]
          this.itemNumber()
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
