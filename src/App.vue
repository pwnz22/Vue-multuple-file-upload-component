<template>
  <div id="app">
    <upload-form></upload-form>
  </div>
</template>

<script>
  import UploadForm from './components/UploadForm'

  export default {
    name: 'app',
    components: {
      UploadForm
    }
  }
</script>
