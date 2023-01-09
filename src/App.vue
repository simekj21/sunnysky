<template>
  <div class="md:flex md:flex-col md:justify-center md:items-center text-center bg-slate-500">
    <FiveDays/>
  </div>
</template>

<script>
import FiveDays from './components/FiveDays.vue'
export default {
  name: 'app',
  components: {
    FiveDays
  }
}
</script>

<style>

</style>