<template>
  <div class="projection">
    <span class="q-mb-md text-weight-medium">{{projection}} months </span>
    <div class="projection__graph flex items-center q-mb-sm">
      <span :style="{ backgroundColor: color, height: '10px', width: barWidth }">
      </span>
    </div>
    <div class="projection__budget" ><span>${{totalBudget * 30}}/month</span></div>
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
    barWidth() {
      const percentage = ((this.projection / this.maxMonths) * 100)
      return `${percentage}%`

    },
    totalBudget() {
      return !!this.increase ? this.budget + (this.budget * (this.increase / 100)) : this.budget
    },
    projection() {
      return Math.round((this.total / this.totalBudget)/30)
    },
    maxMonths() {
      return Math.round((this.total / this.budget)/30)
    }
  }
}
</script>

<style lang="scss">
  .projection__budget {
    font-size: 12px;
    text-align: right;
  }

  .projection__graph {
    background-color: #eee;
  }
</style>

