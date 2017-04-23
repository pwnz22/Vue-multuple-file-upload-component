<template>
  <div class="dragndrop"
       @dragover.prevent="enter"
       @dragenter.prevent="enter"
       @dragleave.prevent="leave"
       @dragend.prevent="leave"
       @drop.prevent="drop"
       :class="{ 'dragndrop--dragged': isDraggedOver }"
  >
    {{ isDraggedOver }}
    <input type="file" name="files[]" id="file" class="dragndrop__input" @change="select" ref="input" multiple>
    <label for="file" class="dragndrop__header dragndrop__header--compact">
      <strong>Drap files here</strong> or click to select files
    </label>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        files: [],
        isDraggedOver: false
      }
    },
    methods: {
      enter () {
        this.isDraggedOver = true
      },
      leave () {
        this.isDraggedOver = false
      },
      drop (e) {
        this.leave()
        console.log(e.dataTransfer.files)
      },
      select (e) {
          console.log(this.$refs.input.files)
      }
    }
  }
</script>

<style lang="scss" scoped>
  $dragndrop-min-height: 200px;

  .dragndrop {
    min-height: $dragndrop-min-height;
    widows: 100%;
    background-color: #f8f8f8;
    position: relative;
    border: 3px dashed rgba(0, 0, 0, .2);

    &--dragged {
      border-color: #333;
    }

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
