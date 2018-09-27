<template>
    <div v-html="html"></div>
</template>

<script>
export default {
  name: 'Slide',
  props: {
    html: String
  }
}
</script>