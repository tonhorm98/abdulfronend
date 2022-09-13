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
  name: 'AdvertisePage',
  data () {
    return {
      selected: [],
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
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        },
        {
          no: 2,
          storeName: '',
          picture: 'https://images.unsplash.com/photo-1481833761820-0509d3217039?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80',
          topic: 'ข้อมูลการเช่าสำนักงานอับดุลราฮิมเพลส',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        },
        {
          no: 3,
          storeName: '',
          picture: 'https://images.unsplash.com/photo-1481833761820-0509d3217039?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80',
          topic: 'ติดต่อพื้นที่เช่าสำนักงานและร้านค้า',
          action: [
            { title: 'View', icon: 'mdi-eye', color: 'green' },
            { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]

        }
      ]
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
        { title: 'View', icon: 'mdi-eye', color: 'green' },
        { title: 'Edit', icon: 'mdi-pencil', color: 'orange' }]
      this.advertisements.push(newAdvertisement)
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
          const result = this.advertisements.filter(item => no.indexOf(item.no) === -1)
          this.advertisements = [...result]
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
