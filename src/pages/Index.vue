<template>
  <q-page class="flex flex-center">
    <q-card class="my-card flex q-ma-md q-pa-lg">
      <q-card-section class="col">
        <div class="text-h4 q-mb-lg">Budget</div>
        <div class="text-h5 q-mb-sm q-mt-md">Savings per day</div>
        <p>How much can you afford to save every day?</p>
        <q-slider
          :value="budget"
          @change="val => { budget = val }"
          :min="1"
          :max="40"
          :label-value="'$' + budget"
          label
          label-always
        />
        <div class="text-h5 q-mb-sm q-mt-md">Length of stay</div>
        <q-input outlined dense v-model="days" />
        <div class="text-h5 q-mb-sm q-mt-md">Number of people</div>
        <q-input outlined dense v-model="people" />
        <div class="text-h5 q-mb-sm q-mt-md">Flights</div>
        <q-radio v-model="flightsRadio" val="500" label="Budget" />
        <q-radio v-model="flightsRadio" val="1000" label="Comfortable" />
        <q-radio v-model="flightsRadio" val="3000" label="Splurge" />
        <q-radio v-model="flightsRadio" val="custom" label="Custom" />
        <div v-if="flightsRadio === 'custom'">
          <q-input outlined dense type="number" v-model.number="flightsInput" />
        </div>
        <div class="text-h5 q-mb-sm q-mt-md">Accommodation</div>
        <q-radio v-model="accommodationRadio" val="50" label="Budget" />
        <q-radio v-model="accommodationRadio" val="150" label="Comfortable" />
        <q-radio v-model="accommodationRadio" val="300" label="Splurge" />
        <q-radio v-model="accommodationRadio" val="custom" label="Custom" />
        <div v-if="accommodationRadio === 'custom'">
          <q-input outlined dense type="number" v-model.number="accommodationInput" />
        </div>
        <div class="text-h5 q-mb-sm q-mt-md">Meals</div>
        <q-radio v-model="mealsRadio" val="50" label="Budget" />
        <q-radio v-model="mealsRadio" val="100" label="Comfortable" />
        <q-radio v-model="mealsRadio" val="150" label="Splurge" />
      </q-card-section>
      <q-card-section class="col">
        <div class="text-h4 q-mb-lg">Savings Projection</div>
        <div class="text-h5 q-mb-sm q-mt-md"></div>
        <div class="text-h6"></div>
        <p>Saving ${{budget}} per day you will be able to afford this vacation in:</p>
        <q-badge class="q-pa-md" color="blue" text-color="black" outline>
          {{projection}} months
        </q-badge>
        <div class="totals q-mt-lg">
          <TableBudget :total="total" />
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
  import TableBudget from "../components/TableBudget";
export default {
  name: 'PageIndex',
  components: {
    TableBudget,
  },
  data () {
    return {
      accommodationInput: '0',
      accommodationRadio: '150',
      budget:  '15',
      days: '7',
      flightsInput: '0',
      flightsRadio: '1000',
      mealsRadio: '100',
      people: '2',
    }
  },
  computed: {
    accommodation() {
      return this.accommodationInput > 0 ?
        parseFloat(this.accommodationInput) :
        parseFloat(this.accommodationRadio) * parseFloat(this.days)
    },
    flights() {
      return this.flightsInput > 0 ?
        parseFloat(this.flightsInput) :
        parseFloat(this.flightsRadio) * parseFloat(this.people)
    },
    meals() {
      return parseFloat(this.mealsRadio) * parseFloat(this.people) * parseFloat(this.days)
    },
    total() {
      return this.accommodation + this.meals + this.flights
    },
    projection() {
      return Math.round((this.total / this.budget)/30)
    },
  }
}
</script>
