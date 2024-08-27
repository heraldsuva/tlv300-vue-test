<script setup>
import titles from '../post-data.json'
import { computed, defineProps } from 'vue'

const props = defineProps({
  domain: String
})

const filteredTitles = computed(() => { 
  return titles.filter((s) => s.Name.toLowerCase().includes(props.domain.toLowerCase()))
})



</script>
<template>
  <div class="root">

  </div>
</template>