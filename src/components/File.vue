<template>
    <div class="dragndrop__file">
        <div class="progress">
            <div class="progress__label">{{ file.file.name }} Filename (x seconds remaining)</div>
            <div class="progress__fill" style="width: 60%;"></div>
            <div class="progress__percentage">
                20%
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['file']
    }
</script>

<style lang="scss">
    .dragndrop {

        &__file {
            margin: 20px;
            margin-top: 0;
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
