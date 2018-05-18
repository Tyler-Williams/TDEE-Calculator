<template>
  <v-app>
    <v-container fluid>
      <v-layout row wrap>
        <v-flex xs6 offset-xs3>
          <v-layout row wrap>
            <v-flex xs6>
              <v-subheader>Sex:</v-subheader>
            </v-flex>
            <v-flex xs6>
              <v-select
                :items="sexs"
                v-model="selectedSex"
                label="Sex"
                single-line
                v-on:input="displaySelection"
              ></v-select>
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      sexs: [
        {text: 'Male', value: 'male'},
        {text: 'Female', value: 'female'}
      ],
      selectedSex: null
    }
  },
  methods: {
    displaySelection () {
      console.log(this.selectedSex)
    }
  }
}
</script>

<style scoped>

</style>
