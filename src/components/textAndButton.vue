<template>
  <td>
    <div>
      <slot></slot>
      <label v-if="false" class="checkbox-container">
        <input type="checkbox">
        <span class="checkbox-custom"></span>
      </label>
    </div>
  </td>
</template>

<script>
export default {
  name: "textAndButton"
}
</script>

<style scoped>

</style>