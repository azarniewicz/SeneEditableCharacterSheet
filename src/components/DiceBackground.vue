<template>
    <div class="dice">
        <span v-if="!editing" v-on:click="toggle">{{ (value ? value : 0) }}</span>
        <div v-else>
            <b-input v-model="value"
                     type="number"
                     custom-class="is-small"
                     @keyup.native.enter="hide"
                     v-click-outside="onClickOutside"
            ></b-input>
        </div>
    </div>
</template>

<script>
    import ClickOutside from 'vue-click-outside'

    export default {
        props: {
            value: {
                type: Number,
                required: false,
                default: 0
            },
            defaultValue: {
                type: Number,
                required: false
            }
        },
        directives: {
            ClickOutside
        },
        data() {
            return {
                editing: false,
                init: false
            }
        },
        methods: {
            toggle() {
                this.editing = true;
            },
            hide() {
                this.editing = false;
                this.init = true;
            },
            onClickOutside() {
                if (this.editing === true && this.init !== true) {
                    this.hide();
                } else {
                    this.init = false
                }
            }
        },
        watch: {
            value(value) {
                this.$emit('input', value);
            },
            defaultValue(val) {
                this.value = val;
            }
        }
    }
</script>

<style lang="scss" scoped>

    .dice {
        background: url("./../assets/dice.png") no-repeat;
        background-size: 100% 100%;
        width: 100%;

        min-height: 25px;

        text-align: center;

        * {
            top: 15%;
        }
    }

    span:hover {
        cursor: pointer;
    }
</style>
