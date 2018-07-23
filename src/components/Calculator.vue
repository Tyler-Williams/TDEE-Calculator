<template>
  <div>
    <img class="logo" src="@/assets/health-graphic.png">
    <v-app>
      <v-container fluid>
        <v-layout row wrap>
          <v-flex xs12 sm10 md6 lg6 offset-xs0 offset-lg3 offset-md3 offset-sm1>
            <v-layout row wrap align-center justify-center>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Units:</v-subheader>
              </v-flex>
              <v-flex xs4 sm6>
                <v-switch
                  v-model="metric"
                  label="Metric"
                  v-on:change="changeUnits"
                ></v-switch>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Sex:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-select
                  :items="sexs"
                  v-model="sex"
                  label="Sex"
                  single-line
                ></v-select>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Age:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-text-field
                  v-model="age"
                  label="Age"
                  name="height-input"
                  id="age"
                  clearable
                  type="number"
                  suffix="years"
                  :disabled="!this.sex"
                ></v-text-field>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Height:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-text-field
                  v-model="height"
                  label="Height"
                  name="height-input"
                  id="height"
                  clearable
                  type="number"
                  :suffix="heightUnits"
                  :disabled="!(this.sex && this.age)"
                ></v-text-field>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Weight:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-text-field
                  v-model="weight"
                  label="Weight"
                  name="weight-input"
                  id="weight"
                  clearable
                  type="number"
                  :suffix="weightUnits"
                  :disabled="!(this.sex && this.height && this.age)"
                ></v-text-field>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Activity Level:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-select
                  :items="activityLevels"
                  v-model="activityLevel"
                  label="Activity Level"
                  single-line
                  :disabled="!(this.sex && this.height && this.weight && this.age)"
                ></v-select>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Personal Goal:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-select
                  :items="goals"
                  v-model="goal"
                  label="Personal Goal"
                  single-line
                  :disabled="!(this.sex && this.height && this.weight && this.activityLevel && this.age)"
                ></v-select>
              </v-flex>
            </v-layout>
            <v-divider></v-divider>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Total TDEE:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-text-field
                  v-model="tdee"
                  name="tdee-total"
                  id="tdee-total"
                  placeholder="Total TDEE"
                  readonly
                  :disabled="!this.tdee"
                  >
                </v-text-field>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex xs6 hidden-xs-only>
                <v-subheader>Goal TDEE:</v-subheader>
              </v-flex>
              <v-flex xs12 sm6>
                <v-text-field
                  v-model="goalTdee"
                  name="tdee-goal"
                  id="tdee-gaol"
                  placeholder="Goal TDEE"
                  readonly
                  :disabled="!this.tdee"
                  >
                </v-text-field>
              </v-flex>
            </v-layout>
          </v-flex>
        </v-layout>
      </v-container>
    </v-app>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      sexs: [
        {text: 'Male', value: 66},
        {text: 'Female', value: 655}
      ],
      activityLevels: [
        {text: 'Sedentary', value: 1.2},
        {text: 'Lightly Active', value: 1.375},
        {text: 'Moderately Active', value: 1.55},
        {text: 'Very Active', value: 1.725},
        {text: 'Extremely Active', value: 1.9}
      ],
      goals: [
        {text: 'Gain Weight', value: 1.2},
        {text: 'Maintain Weight', value: 1},
        {text: 'Loose Weight', value: 0.7}
      ],
      sex: null,
      activityLevel: null,
      goal: null,
      metric: true,
      weightUnits: 'KG',
      heightUnits: 'cm',
      height: null,
      weight: null,
      age: null
    }
  },
  methods: {
    changeUnits () {
      if (this.metric) {
        this.weightUnits = 'KG'
        this.heightUnits = 'cm'
      } else {
        this.weightUnits = 'LBS'
        this.heightUnits = 'in'
      }
    },
    calculateMen () {
      return (this.sex + (13.7 * this.calculatedWeight) + (5 * this.calculatedHeight) - (6.8 * parseInt(this.age))) * this.activityLevel
    },
    calculateWoman () {
      return (this.sex + (9.6 * this.calculatedWeight) + (1.8 * this.calculatedHeight) - (4.7 * parseInt(this.age))) * this.activityLevel
    }
  },
  computed: {
    tdee () {
      const tdee = this.sex === 66 ? this.calculateMen() : this.calculateWoman()
      return tdee || null
    },
    calculatedHeight () {
      return this.metric ? this.height : Math.floor(this.height * 2.54)
    },
    calculatedWeight () {
      return this.metric ? this.weight : Math.floor(this.weight * 2.2)
    },
    goalTdee () {
      return this.tdee * this.goal || null
    }
  }
}
</script>

<style scoped>
.logo {
  width: 66%;
}
</style>
