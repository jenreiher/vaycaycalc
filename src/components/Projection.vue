<template>
  <div class="budget-projection flex flex-center column">
    <span class="q-mb-sm text-weight-medium">${{totalBudget}} per day:</span>
    <q-badge class="q-pa-sm" :color="color" text-color="black" outline>
      {{projection}} months
    </q-badge>
  </div>

</template>
<script>
export default {
  name: 'Projection',
  props: {
    budget: {
      type: Number,
      required: true,
    },
    color: {
      type: String,
      required: true,
    },
    increase: {
      type: Number,
      default: 0,
    },
    total: {
      type: Number,
      required: true,
    }
  },
  computed: {
    totalBudget() {
      return this.budget + this.increase
    },
    projection() {
      return Math.round((this.total / this.totalBudget)/30)
    },
  }
}
</script>
