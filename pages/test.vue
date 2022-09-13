<template>
  <div>
    <v-btn @click="testfn">
      test
    </v-btn>
    <!-- <v-text-field
      v-model="testdata"
      required
      outlined
      dense
    /> -->
    <h4 v-for="(item, index) in testdata.detail" :key="index">
      {{ item.NewsSubject }} <hr>
    </h4>
  </div>
</template>

<script>
export default {
  name: 'TestPage',
  data () {
    return {
      testdata: ''
    }
  },
  methods: {
    async testfn () {
      const getsth = await this.$axios.$get('https://ocsc9.thaijobjob.com:8101/news')
      console.log(getsth)
      this.testdata = getsth
    }
  }
}
</script>

<style>

</style>
