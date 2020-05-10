<template>
  <q-table
    :title="`Budget Breakdown`"
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
            <q-radio v-model="accommodationRadio" val="budget" label="Budget" />
            <q-radio v-model="accommodationRadio" val="comfortable" label="Comfortable" />
            <q-radio v-model="accommodationRadio" val="splurge" label="Splurge" />
            <div class="flex">
              <q-radio v-model="accommodationRadio" val="custom" label="Custom" />
              <div v-if="accommodationRadio === 'custom'">
                <q-input outlined dense v-model.number="accommodationInput" type="number" prefix="$"  class="q-ml-sm" />
              </div>
            </div>
          </template>
          <template v-if="props.row.name === 'Transportation'">
            <q-radio v-model="flightsRadio" val="budget" label="Budget" />
            <q-radio v-model="flightsRadio" val="comfortable" label="Comfortable" />
            <q-radio v-model="flightsRadio" val="splurge" label="Splurge" />
            <div class="flex">
              <q-radio v-model="flightsRadio" val="custom" label="Custom" />
              <div v-if="flightsRadio === 'custom'">
                <q-input outlined dense v-model.number="flightsInput" type="number" prefix="$" class="q-ml-sm"/>
            </div>
            </div>
          </template>
          <template v-if="props.row.name === 'Meals'">
            <q-radio v-model="mealsRadio" val="budget" label="Budget" />
            <q-radio v-model="mealsRadio" val="comfortable" label="Comfortable" />
            <q-radio v-model="mealsRadio" val="splurge" label="Splurge" />
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
    destination: {
      type: String,
      required: true
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
      accommodationRadio: 'comfortable',
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
      flightsRadio: 'comfortable',
      mealsRadio: 'comfortable',
    }
  },
  computed: {
    accommodation() {
      return this.accommodationRadio === 'custom' ?
        parseFloat(this.accommodationInput) :
        this.getCostEstimate('accommodation', this.accommodationRadio) * parseFloat(this.days)
    },
    flights() {
      return this.flightsRadio === 'custom' ?
        parseFloat(this.flightsInput) :
        this.getCostEstimate('flights', this.flightsRadio) * parseFloat(this.people)
    },
    meals() {
      return this.getCostEstimate('meals', this.mealsRadio) * this.people * this.days
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
          name: 'Transportation',
          total: `$${this.flights}`
        },
        {
          name: 'Meals',
          total: `$${this.meals}`,
        },
      ]
    }
  },
  methods: {
    getCostEstimate: function(type, budget) {
      switch (this.destination) {
        case 'France':
          if(type === 'accommodation') {
            switch (budget) {
              case 'budget':
                return 100;
              case 'comfortable':
                return 375;
              case 'splurge':
                return 1000;
              default: console.log(`Sorry, no data found for ${budget}.`);
            }
          }
          if(type === 'flights') {
            switch (budget) {
              case 'budget':
                return 500;
              case 'comfortable':
                return 800;
              case 'splurge':
                return 2000;
              default: console.log(`Sorry, no data found for ${budget}.`);
            }
          }
          if(type === 'meals') {
            switch (budget) {
              case 'budget':
                return 25;
              case 'comfortable':
                return 45;
              case 'splurge':
                return 200;
              default: console.log(`Sorry, no data found for ${budget}.`);
            }
          }
          break;
        case 'Mexico':
          if(type === 'accommodation') {
            switch (budget) {
              case 'budget':
                return 20;
              case 'comfortable':
                return 55;
              case 'splurge':
                return 170;
              default: console.log(`Sorry, no data found for ${budget}.`);
            }
          }
          if(type === 'flights') {
            switch (budget) {
              case 'budget':
                return 450;
              case 'comfortable':
                return 700;
              case 'splurge':
                return 1500;
              default: console.log(`Sorry, no data found for ${budget}.`);
            }
          }
          if(type === 'meals') {
            switch (budget) {
              case 'budget':
                return 10;
              case 'comfortable':
                return 25;
              case 'splurge':
                return 45;
              default: console.log(`Sorry, no data found for ${budget}.`);
            }
          }
          break;
        default:
          console.log(`Sorry, no data found for ${destination}.`);
      }
    }
  }
}
</script>
