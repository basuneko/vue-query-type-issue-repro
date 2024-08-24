<script setup lang="ts">
import { ref } from 'vue';
import { queryOptions, useQuery } from '@tanstack/vue-query';

const plainOptions = queryOptions({
  queryKey: ['bacon'],
  queryFn: () => Promise.resolve(1)
})

console.log('plainOptions.queryKey', plainOptions.queryKey)
// tsc
// expected: DataTag<>
// actual: DataTag<>
// runtime
// expected: ['bacon']
// actual: ['bacon']

const refOptions = queryOptions(ref({
  queryKey: ['bacon'],
  queryFn: () => Promise.resolve(1)
}))

// refOptions is a Ref object

console.log('refOptions.queryKey', refOptions.queryKey)
// tsc
// expected: error
// actual: DataTag<>
// runtime
// expected: undefined
// actual: undefined

console.log('refOptions.value.queryKey', refOptions.value.queryKey)
// tsc
// expected: DataTag
// actual: error
// runtime
// expected: ['bacon']
// actual ['bacon']



// This works
const query1 = useQuery(refOptions)

// This passes tsc but fails in runtime with `TypeError: Cannot read properties of undefined (reading 'enabled')`
const query2 = useQuery({
  ...refOptions,
  placeholderData: 0
})
</script>

<template>
  <div></div>
</template>
