<template >
  <q-page class="flex flex-center">
    <q-card class="budget-card flex q-ma-md q-pa-lg">
      <q-card-section class="col">
        <div class="text-h4 q-mb-lg">Budget</div>
        <div class="text-h5 q-mb-sm q-mt-md">Savings per day</div>
        <p class="q-mb-lg">How much can you afford to save every day?</p>
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
        <q-input outlined dense type="number" v-model.number="days" />
        <div class="text-h5 q-mb-sm q-mt-md">Number of people</div>
        <q-input outlined dense type="number" v-model.number="people" />
      </q-card-section>
      <q-card-section class="col">
        <div class="text-h4 q-mb-lg">Savings Plan</div>
        <div class="text-h5 q-mb-sm q-mt-md"></div>
        <div class="text-h6"></div>
        <div class="flex justify-between">
          <Projection color="green" :budget="budget" :total="total"/>
          <Projection color="orange" :budget="budget" :increase="2" :total="total"/>
          <Projection color="red" :budget="budget" :increase="5" :total="total"/>
        </div>
        <div class="totals q-mt-lg">
          <TableBudget @update-total="onUpdateTotal" :days="days" :people="people" />
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
      budget:  10,
      days: 7,
      people: 2,
      total: 0,
    }
  },
  computed: {

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
