<template >
  <q-page class="flex flex-center">
    <q-card class="budget-card flex q-ma-md q-pa-lg">
      <q-card-section class="budget col">
        <div class="text-h4 q-mb-lg">Budget</div>

        <div class="text-h5 q-mb-sm q-mt-md">Destination</div>
        <q-select outlined v-model="destination" :options="options" dense />

        <div class="text-h5 q-mb-sm q-mt-md">Savings per day</div>
        <p class="q-mb-sm">How much can you afford to save every day?</p>
        <div class="daily-budget flex column">
          <q-radio v-model="budgetRadio" :val="5" label="A latte is not a lot, eh ($5)" />
          <q-radio v-model="budgetRadio" :val="10" label="Let's talk about tacos ($10)" />
          <q-radio v-model="budgetRadio" :val="20" label="Pizza please, duh! ($20)" />
          <div class="flex">
            <q-radio v-model="budgetRadio" val="custom" label="Custom" />
            <div v-if="budgetRadio === 'custom'">
              <q-input outlined dense v-model.number="budgetInput" type="number" prefix="$" class="q-ml-sm"/>
            </div>
          </div>
        </div>
        <div class="text-h5 q-mb-sm q-mt-md">Length of stay</div>
        <q-input outlined dense type="number" v-model.number="days" />
        <div class="text-h5 q-mb-sm q-mt-md">Number of people</div>
        <q-input outlined dense type="number" v-model.number="people" />
      </q-card-section>
      <q-card-section class="savings col">
        <div class="text-h4 q-mb-lg">Savings Plan</div>
        <div class="text-h5 q-mb-sm q-mt-md"></div>
        <div class="text-h6"></div>
        <div class="projections">
          <Projection color="#6aa6e2" :budget="budget" :total="total"/>
          <Projection color="#e2a66a" :budget="budget" :increase="20" :total="total"/>
          <Projection color="#2ad62a" :budget="budget" :increase="50" :total="total"/>
        </div>
        <div class="totals q-mt-lg">
          <TableBudget @update-total="onUpdateTotal" :destination="destination" :days="days" :people="people" />
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import Projection from "../components/Projection";
import TableBudget from "../components/TableBudget";

export default {
  name: 'PageIndex',
  components: {
    Projection,
    TableBudget,
  },
  data () {
    return {
      budgetRadio: 10,
      budgetInput: 15,
      days: 7,
      people: 2,
      total: 0,
      destination: 'France',
      options: ['Mexico', 'France']
    }
  },
  computed: {
    budget() {
      return this.budgetRadio === 'custom' ? this.budgetInput : this.budgetRadio
    }
  },
  methods: {
    onUpdateTotal: function (total) {
      this.total = total
    },
  }
}
</script>

<style lang="scss">
  .budget-card {
    width: 750px;
  }
</style>
