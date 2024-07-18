<template>
  <table class="table">
    <thead>
      <tr>
        <th v-for="(value, key) in schema.properties" :key="key">
          {{ value.title || key }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(row, index) in data" :key="index">
        <td v-for="(value, key) in schema.properties" :key="key">
          {{ row[key] }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps<{
  schema: any;
  data: Array<any>;
}>();
</script>

<style>
.table {
  width: 100%;
  border-collapse: collapse;
}
.table th, .table td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}
</style>
