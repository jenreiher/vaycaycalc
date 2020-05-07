<template>
  <q-table
    title="Budget Breakdown"
    :data="data"
    :columns="columns"
    row-key="name"
    hide-bottom
  >
    <template v-slot:body="props">
      <q-tr :props="props">
        <q-td auto-width>
          <q-btn
            size="sm"
            color="accent"
            dense
            @click="props.expand = !props.expand"
            :icon="props.expand ? 'remove' : 'add'"
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
          <div class="text-left">This is expand slot for row above: {{ props.row.name }}.</div>
        </q-td>
      </q-tr>
    </template>
    <template v-slot:bottom-row>
      <q-tr class="bg-grey-2 text-weight-medium">
        <q-td>
          Total
        </q-td>
        <q-td/>
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
    total: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      columns: [
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
      data: [
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
