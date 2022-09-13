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
      Add new
    </v-btn>
    <v-dialog v-model="showDialog" width="1000" persistent>
      <v-card>
        <v-card-title class="blue darken-4 white--text">
          Add New Advertisement เพิ่มข้อมูลประชาสัมพันธ์
        </v-card-title>
        <v-form ref="inputForm" class="px-4 py-6 h-100 mx-5" @submit.prevent="submitForm">
          <v-row>
            <v-col cols="5">
              picture
            </v-col>
            <v-col cols="7">
              <v-row><h4>หัวข้อเรื่องประชาสัมพันธ์</h4></v-row>
              <v-row>
                <v-text-field
                  v-model="topic"
                  required
                  outlined
                  dense
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
                  v-model="detail"
                  outlined
                  auto-grow
                  rows="7"
                  row-height="30"
                  dense
                  placeholder="GPF Property Management Limited บริษัทจีพีเอฟ พร็อพเพอร์ตี้แมเนจเม้นท์ จำกัด - 990 อาคารอับดุลราฮิม ชั้น 21 ถนนพระราม 4 แขวงสีลม เขตบางรัก กรุงเทพ 10500- Tel. 02-636-1990- Email : mkt@gpfpro.co.th"
                />
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
      topic: '',
      detail: ''
    }
  },
  methods: {
    submitForm () {
      const advertisementDetail = {}
      advertisementDetail.topic = this.topic
      advertisementDetail.detail = this.detail
      this.$emit('addAdvertisement', advertisementDetail)
      this.$swal.fire({
        icon: 'success',
        title: 'Your work has been saved',
        showConfirmButton: true,
        timer: 2000
      })
      this.$refs.inputForm.reset()
      this.showDialog = false
    }
  }
}
</script>

<style>
.require{
    color: red;
}
</style>
