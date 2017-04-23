<template>
  <div class="dragndrop">
    <input type="file" name="files[]" id="file" class="dragndrop__input" multiple>
    <label for="file" class="dragndrop__header dragndrop__header--compact">
      <strong>Drap files here</strong> or click to select files
    </label>
  </div>
</template>

<script>
  export default {}
</script>

<style lang="scss" scoped>
  $dragndrop-min-height: 200px;

  .dragndrop {
    min-height: $dragndrop-min-height;
    widows: 100%;
    background-color: #f8f8f8;
    position: relative;
    border: 3px dashed rgba(0, 0, 0, .2);

    &__input {
      display: none;
    }

    &__header {
      display: block;
      font-size: 1.1em;
      color: #555;
      vertical-align: middle;
      text-align: center;
      margin: ($dragndrop-min-height / 2);

      &:hover {
        text-decoration: underline;
        cursor: pointer;
      }

      &--compact {
        margin: ($dragndrop-min-height / 4) 20px;
      }
    }
  }
</style>
