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
    <v-dialog v-model="showDialog" width="600" persistent>
      <v-card>
        <v-card-title class="blue darken-4 white--text">
          will be edited later, need to leart about ref first
        </v-card-title>
        <v-form ref="inputForm" class="px-4 py-6 h-100 mx-5" @submit.prevent="submitForm">
          <v-row>
            <v-col cols="12">
              <v-row><h2><b class="require">*</b>ชื่อ-นามสกุล</h2></v-row>
              <v-row>
                <v-text-field
                  v-model="fullName"
                  required
                  outlined
                  dense
                  placeholder="สมชาย สายชม"
                />
              </v-row>

              <v-row><h2><b class="require">*</b>อีเมล์</h2></v-row>
              <v-row>
                <v-text-field
                  v-model="email"
                  required
                  outlined
                  dense
                  placeholder="somchai.s@gmail.com"
                />
              </v-row>
              <v-row><h2><b class="require">*</b>เบอร์โทรศัพท์</h2></v-row>
              <v-row>
                <v-text-field
                  v-model="tel"
                  required
                  outlined
                  dense
                  placeholder="0900000000"
                />
              </v-row>
              <v-row><h2><b class="require">*</b>รหัสผ่าน</h2></v-row>
              <v-row>
                <v-text-field
                  v-model="passwd"
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
      fullName: '',
      email: '',
      phoneNumber: '',
      passwd: '',
      tel: '',
      value: ''

    }
  },
  methods: {
    submitForm () {
      // console.log(this.promotion)
      const userDetail = {}
      userDetail.fullName = this.fullName
      userDetail.email = this.email
      userDetail.tel = this.tel
      userDetail.passwd = this.passwd

      this.$emit('addUser', userDetail)
      this.$swal.fire({
        icon: 'success',
        title: 'Your work has been saved',
        showConfirmButton: true,
        timer: 2000
      })
      this.$refs.inputForm.reset() // try to reset form
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
