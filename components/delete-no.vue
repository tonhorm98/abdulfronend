<template>
  <div>
    <v-btn
      color="error"
      class="mx-3"
      @click="deleteItemConfirm (selected)"
    >
      <v-icon left>
        mdi-delete
      </v-icon>
      Delete
    </v-btn>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['selected'],
  data () {
    return {
    }
  },
  methods: {
    deleteItemConfirm (selected) {
      const no = []
      for (let i = 0; i < selected.length; i += 1) {
        no.push(selected[i].no)
      }
      // console.log(no)
      this.$emit('deleteByNo', no)
    }
  }
}
</script>

<style>

</style>
