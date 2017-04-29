<template>
    <div class="dragndrop__file">
        <div class="progress">
            <div class="progress__label">{{ file.file.name }} Filename (x seconds remaining)</div>

            <div class="progress__fill"
                 :style="{ width: file.progress + '%' }"
                 :class="{ 'progress__fill--finished': file.finished, 'progress__fill--failed': file.failed || file.cancelled }"></div>

            <div class="progress__percentage">
                <span v-if="file.failed">Failed</span>
                <span v-if="file.finished">Complete</span>
                <span v-if="file.cancelled">Complete</span>

                <span v-if="!file.finished && !file.failed && !file.cancelled">
                    {{ file.progress }}%
                </span>
            </div>
        </div>
    </div>
</template>

<script>
    import eventHub from '../events'
    export default {
        props: ['file'],

        methods: {
            updateFileProgress(fileObject, e) {
                if (!e.lengthComputable) {
                    return
                }
                fileObject.loadedBytes = e.loaded
                fileObject.totalBytes = e.total
                fileObject.progress = Math.ceil((e.loaded / e.total) * 100)
            }
        },

        mounted() {
            eventHub.$on('progress', (fileObject, e) => {
                this.updateFileProgress(fileObject, e)
            })

            eventHub.$on('finished', (fileObject, e) => {
                if (fileObject.id === this.file.id) {
                    this.file.finished = true
                }
            })

            eventHub.$on('failed', (fileObject, e) => {
                if (fileObject.id === this.file.id) {
                    this.file.failed = true
                }
            })
        }
    }
</script>

<style lang="scss">
    .dragndrop {

        &__file {
            margin: 0 20px 20px;
            font-size: 0.9em;
            position: relative;
        }
    }

    .progress {
        box-shadow: inset 0 1px 2px rgba(0, 0, 0, .1);
        border-radius: 3px;
        background-color: #f5f5f5;
        height: 35px;

        &__label,
        &__percentage {
            position: absolute;
            color: #333;
            top: 50%;
            transform: translate(0, -50%);
            margin-left: 10px;
        }

        &__percentage {
            right: 0;
            margin-right: 10px;
        }

        &__fill {
            box-sizing: border-box;
            padding: 10px;
            border-radius: 3px;
            background-color: #42b983;
            height: 100%;
            box-shadow: inset 0 -1px rgba(0, 0, 0, .15);
            transition: width 500ms ease;
            opacity: .7;

            &--finished {
                opacity: 1;
            }

            &--failed {
                transition: none;
                width: 100% !important;
                background-color: #f66;
            }
        }
    }
</style>
