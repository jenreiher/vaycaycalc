<template>
  <q-table
    title="Budget Breakdown"
    :data="tableData"
    :columns="columns"
    row-key="name"
    hide-bottom
  >
    <template v-slot:body="props">
      <q-tr :props="props">
        <q-td auto-width>
          <q-btn
            size="sm"
            color="blue"
            dense
            @click="props.expand = !props.expand"
            :icon="props.expand ? 'remove' : 'settings'"
          />
        </q-td>
        <q-td
          v-for="col in props.cols"
          :key="col.name"
          :props="props"
        >
          {{ col.value }}
        </q-td>
      </q-tr>
      <q-tr v-show="props.expand" :props="props">
        <q-td colspan="100%">
          <template v-if="props.row.name === 'Accommodation'">
            <q-radio v-model="accommodationRadio" :val="50" label="Budget" />
            <q-radio v-model="accommodationRadio" :val="150" label="Comfortable" />
            <q-radio v-model="accommodationRadio" :val="300" label="Splurge" />
            <div class="flex">
              <q-radio v-model="accommodationRadio" val="custom" label="Custom" />
              <div v-if="accommodationRadio === 'custom'">
                <q-input outlined dense v-model="accommodationInput"  class="q-ml-sm" />
              </div>
            </div>
          </template>
          <template v-if="props.row.name === 'Flights'">
            <q-radio v-model="flightsRadio" :val="500" label="Budget" />
            <q-radio v-model="flightsRadio" :val="1000" label="Comfortable" />
            <q-radio v-model="flightsRadio" :val="3000" label="Splurge" />
            <div class="flex">
              <q-radio v-model="flightsRadio" val="custom" label="Custom" />
              <div v-if="flightsRadio === 'custom'">
                <q-input outlined dense v-model="flightsInput" class="q-ml-sm"/>
            </div>
            </div>
          </template>
          <template v-if="props.row.name === 'Meals'">
            <q-radio v-model="mealsRadio" :val="50" label="Budget" />
            <q-radio v-model="mealsRadio" :val="100" label="Comfortable" />
            <q-radio v-model="mealsRadio" :val="150" label="Splurge" />
          </template>
        </q-td>
      </q-tr>
    </template>
    <template v-slot:bottom-row>
      <q-tr class="bg-grey-2 text-weight-medium">
        <q-td colspan="2">
          Estimated Total
        </q-td>
        <q-td align="right">
          ${{total}}
        </q-td>
      </q-tr>
    </template>
  </q-table>
</template>

<script>
export default {
  name: 'TableBudget',
  props: {
    days: {
      type: [String, Number],
      required: true,
    },
    people: {
      type: [String, Number],
      required: true,
    },
  },
  mounted() {
    this.$emit('update-total', this.total)
  },
  data() {
    return {
      accommodationInput: 0,
      accommodationRadio: 150,
      columns: [
        {},
        {
          name: 'expense',
          required: true,
          label: 'Expense',
          align: 'left',
          field: row => row.name,
          format: val => `${val}`,
        },
        {
          name: 'total',
          label: 'Total',
          field: 'total',
          sortable: true,
        }
      ],
      flightsInput: 0,
      flightsRadio: 1000,
      mealsRadio: 100,
    }
  },
  computed: {
    accommodation() {
      return this.accommodationRadio === 'custom' ?
        parseFloat(this.accommodationInput) :
        parseFloat(this.accommodationRadio) * parseFloat(this.days)
    },
    flights() {
      return this.flightsRadio === 'custom' ?
        parseFloat(this.flightsInput) :
        parseFloat(this.flightsRadio) * parseFloat(this.people)
    },
    meals() {
      return this.mealsRadio * this.people * this.days
    },
    total() {
      const amount = this.accommodation + this.meals + this.flights
      this.$emit('update-total', amount)
      return amount
    },
    tableData() {
      return [
        {
          name: 'Accommodation',
          total: `$${this.accommodation}`
        },
        {
          name: 'Flights',
          total: `$${this.flights}`
        },
        {
          name: 'Meals',
          total: `$${this.accommodation}`,
        },
      ]
    }
  }
}
</script>
