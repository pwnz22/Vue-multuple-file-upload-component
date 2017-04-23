<template>
    <div>
        <file v-for="(file, index) in files" :key="index" :file="file"></file>
    </div>
</template>

<script>
    import File from './File'
    export default {
        props: ['files'],
        components: {File}
    }
</script>
