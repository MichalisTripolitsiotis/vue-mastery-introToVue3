<template>
  <ul>
              <li v-for="detail in details" :key="detail">{{detail}}</li>
            </ul>
</template>

<script>
export default {
 props: {
    details: {
      type: Array,
      required: true
    }
  },
}
</script>

<style>

</style>