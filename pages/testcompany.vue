<!-- eslint-disable unicorn/prefer-includes -->
<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
      >
        <v-card>
          <!-- เดี๋ยวอาจปรับอีก  -->
          <h1 class="ma-2 pa-2" style="display:inline;">
            บริษัทคู่ค้า
          </h1>
          <h2 style="display:inline;" class="font-weight-light grey--text text--darken-2">
            COMPANY
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
              <add-new-company @addCompany="addData($event)" />
              <!-- edit here for view and edit (maybe for create later) -->
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
                          <v-row><h4>ชื่อบริษัท(TH)</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.nameTH"
                              required
                              outlined
                              dense
                              placeholder="คาเฟ่ อเมซอน"
                              :disabled="isDisable"
                            />
                          </v-row>
                          <v-row><h4>ชื่อบริษัท(EN)</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.nameEN"
                              required
                              outlined
                              dense
                              placeholder="Cafe Amazon"
                              :disabled="isDisable"
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
                          <v-row><h4>taxId</h4></v-row>
                          <v-row>
                            <v-text-field
                              v-model="editedItem.taxId"
                              required
                              outlined
                              dense
                              :disabled="isDisable"

                              placeholder="123456789"
                            />
                          </v-row>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>
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
                </v-card>
              </v-dialog>
            </v-col>
            <!-- page shop มีปุ่ม delete -->
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
                :items="companies"
                item-key="companyId"
                class="elevation-1 mx-3"
              >
                <!-- ประมาณว่าสร้าง el แล้วใส่ลงใน column action-->
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
                <!-- ประมาณว่าสร้าง el รูปภาพ แล้วใส่ลงใน value picture-->
                <!-- ประมาณว่าแปลงเป็น el รูปภาพก่อน แล้วค่อยเอาไปแทนในค่าที่ value:'picture'-->

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
//  blue darken-4 white--text
export default {
  name: 'TestCompanyPage',
  data () {
    return {
      showDialog: false,
      selected: [],
      isDisable: false,
      isView: false,
      editedIndex: -1, // ยังงงๆอยู่ ไม่มีก็ได้มั้ง
      headers: [ // ประมาณ column แรก
        {
          text: 'No.', // ชื่อ column แรก
          align: 'start',
          sortable: false,
          value: 'companyId' // ชื่อตัวแปรที่เอามาใส่ใน column แรก
        },
        { text: 'รูป', value: 'picture' },
        { text: 'บริษัท(TH)', value: 'nameTH' },
        { text: 'บริษัท(EN)', value: 'nameEN' },
        { text: 'taxId', value: 'taxId' },
        { text: 'Action', value: 'action' }
      ],
      companies: [],
      actions: [
        { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }
      ],
      editedItem: {
        companyId: '',
        picture: '',
        nameTH: '',
        nameEN: '',
        detail: '',
        taxId: ''
      }
    }
  },
  computed: {
    formTitle () {
      if (this.isView === true) { return 'ดูรายละเอียด' }
      //   return this.editedIndex === -1 ? 'Add New Promotion เพิ่มโปรโมชั่น' : 'แก้ไขโปรโมชั่น'
      return 'แก้ไขข้อมมูลบริษัทร่วมค้า'
    }
  },
  mounted () {
    this.initialize()
  },
  methods: {
    async addData (companyDetail) {
      const newCompany = {}
      newCompany.nameTH = companyDetail.nameTH
      newCompany.nameEN = companyDetail.nameEN
      newCompany.detail = companyDetail.detail
      newCompany.taxId = companyDetail.taxId
      // newCompany.companyId = this.companies[this.companies.length - 1].companyId + 1
      // newCompany.action = [
      //   { title: 'View', icon: 'mdi-eye', color: 'green', doMethod: 'view' },
      //   { title: 'Edit', icon: 'mdi-pencil', color: 'orange', doMethod: 'edit' }]
      // this.companies.push(newCompany)
      await this.$axios.$post('http://localhost:8972/company/create', { data: newCompany }) // create
      this.initialize()
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
      }).then(async (result) => {
        if (result.isConfirmed) {
          const companyId = []
          for (let i = 0; i < selected.length; i += 1) {
            companyId.push(selected[i].companyId)
          }
          // eslint-disable-next-line unicorn/prefer-includes
          // const result = this.companies.filter(item => companyId.indexOf(item.companyId) === -1)
          // this.companies = [...result]
          await this.$axios.$post('http://localhost:8972/company/delete', { data: companyId }) // create
          this.selected = []
          this.initialize()
          this.$swal.fire(
            'Deleted!',
            'Your selected items have been deleted.',
            'success'
          )
        }
      })
    },
    // new
    menuActionClick (action, item) {
      if (action === 'view') {
        this.isView = true
        this.isDisable = true
        this.editedIndex = this.companies.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.showDialog = true
      } else if (action === 'edit') {
        this.isView = false
        this.isDisable = false
        this.editedIndex = this.companies.indexOf(item)
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
    async save () {
      if (this.editedIndex > -1) {
        // case when edit
        Object.assign(this.companies[this.editedIndex], this.editedItem)
        await this.$axios.$post('http://localhost:8972/company/update', { data: this.companies[this.editedIndex] }) // edit data
      } else {
        // case when create data may be can delete later
        // if we use dialog for create view delete so we need to fix here later
        // this.companies.push(this.editedItem)
      }
      this.close()
    },
    async initialize () {
      const getcompanies = await this.$axios.$get('http://localhost:8972/company')
      this.companies = getcompanies
      for (let i = 0; i < this.companies.lenth; i += 1) {
        this.companyList.push(this.companies[i].nameEN)
      }
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
