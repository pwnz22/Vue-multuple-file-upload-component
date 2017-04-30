<template>
    <div class="dragndrop"
         @dragover.prevent="enter"
         @dragenter.prevent="enter"
         @dragleave.prevent="leave"
         @dragend.prevent="leave"
         @drop.prevent="drop"
         :class="{ 'dragndrop--dragged': isDraggedOver }"
    >
        <input type="file" name="files[]" id="file" class="dragndrop__input" @change="select" ref="input" multiple>

        <label for="file" class="dragndrop__header" :class="{ 'dragndrop__header--compact': files.length >= 1 }">
            <strong>Drap files here</strong> or click to select files
        </label>

        <uploads :files="files"></uploads>
    </div>
</template>

<script>
    import axios from 'axios'
    import Uploads from './Uploads.vue'
    import eventHub from '../events'

    export default {
        components: {Uploads},
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
                this.addFiles(e.dataTransfer.files)
            },
            select (e) {
                this.addFiles(this.$refs.input.files)
            },
            addFiles(files) {
                let i, file

                for (i = 0; i < files.length; i++) {
                    file = files[i]

                    this.storeMeta(file).then(fileObject => {
                        console.log(fileObject.id)
                    }, fileObject => {
                        console.log(fileObject.id)
                    })
                }
            },
            storeMeta(file) {
                let fileObject = this.generateFileObject(file)
                this.upload(fileObject)

                return new Promise((resolve, reject) => {
                    axios.post('http://fileupload/store.php/', {
                        name: file.name
                    }).then(response => {
                        fileObject.id = response.data.data.id
                        resolve(fileObject)
                    }, () => {
                        reject(fileObject)
                    })
                })
            },
            upload(fileObject) {
                const form = new FormData()

                form.append('file', fileObject.file)
                form.append('id', fileObject.id)

                eventHub.$emit('init')

                axios.post('http://fileupload/upload.php', form, {
                    before: (xhr) => {
                        fileObject.xhr = xhr
                    },
                    onUploadProgress: (e) => {
                        eventHub.$emit('progress', fileObject, e)
                    }
                }).then(() => {
                    eventHub.$emit('finished', fileObject)
                }, () => {
                    eventHub.$emit('failed', fileObject)
                })
            },
            generateFileObject(file) {
                let fileObjectIndex = this.files.push({
                        id: null,
                        file: file,
                        progress: 0,
                        failed: false,
                        loadedBytes: 0,
                        totalBytes: 0,
                        timeStarted: (new Date).getTime(),
                        secondsRemaining: 0,
                        finished: false,
                        cancelled: false,
                        xhr: null
                    }) - 1

                return this.files[fileObjectIndex]
            }
        }
    }
</script>

<style lang="scss" scoped>
    $dragndrop-min-height: 200px;

    .dragndrop {
        min-height: $dragndrop-min-height;
        width: 100%;
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
