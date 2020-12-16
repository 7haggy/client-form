<template>
    <div class="select" ref="select" :class="{ active: toggle }" @click="toggle = !toggle">
        <div class="value" v-if="isMulti">
            <span v-for="(val, index) in value" :key="'val' + index" class="active">{{ val }}</span>
        </div>
        <div class="value" v-else>
            <span>{{ value }}</span>
        </div>
        <div class="dropdown" v-if="toggle">
            <ul class="list">
                <li
                    class="option"
                    :class="{ active: value.indexOf(option) != -1 }"
                    v-for="(option, index) in options"
                    :key="index"
                    @click="isMulti ? updateValue(option) : value != option ? (value = option) : (value = '')"
                >
                    {{ option }}
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
    export default {
        name: "customSelect",
        props: {
            options: {
                type: Array,
                default: () => {
                    return [];
                },
            },
            isMulti: {
                type: Boolean,
                default: false,
            },
        },
        data() {
            return {
                toggle: false,
                value: this.isMulti ? [] : "",
            };
        },
        watch: {
            value() {
                this.$emit("input", this.value);
            },
        },
        created() {
            document.addEventListener("click", this.clickOutside);
        },
        destroyed() {
            document.removeEventListener("click", this.clickOutside);
        },
        methods: {
            clickOutside(e) {
                if (!this.$el.contains(e.target)) {
                    this.toggle = false;
                }
            },
            updateValue(option) {
                if (this.value.indexOf(option) == -1) {
                    this.value.push(option);
                } else {
                    for (let i = 0; i < this.value.length; i++) {
                        if (this.value[i] == option) {
                            this.value.splice(i, 1);
                        }
                    }
                }
            },
        },
    };
</script>
<style lang="sass" scoped>
    .select
        position: relative
        min-width: 194px
        height: 40px
        border: 1px solid #f2f2f2
        box-sizing: border-box
        box-shadow: 0 2px 6px rgba(0,0,0,.05)
        border-radius: 5px
        margin-top: 5px
        cursor: pointer

        &:after
            content: url('../assets/arrow.svg')
            width: 15px
            position: absolute
            right: 8px
            top: 11px
            transition: all 0.2s

        &.active
            &:after
                transform: rotate(180deg)

        .value
            max-width: 150px
            height: 100%
            display: flex
            align-items: center
            padding-left: 10px
            overflow: auto
            white-space: nowrap
            font-size: 14px
            &::-webkit-scrollbar-track
                border-radius: 10px
                background-color: #F5F5F5
            

            &::-webkit-scrollbar
                height: 6px
                background-color: #F5F5F5
            

            &::-webkit-scrollbar-thumb
                border-radius: 10px
                background-color: #cac9c9
            
            .active
                padding: 3px 7px
                color: white
                background-color: #f47c69
                border-radius: 12px
                margin-right: 7px
            

        .dropdown
            width: 100%
            position: absolute
            z-index: 100
            top: 30px
            background: white
            box-shadow: 0 5px 7px rgba(0,0,0,.05)
            margin-top: 5px
            border-bottom-right-radius: 5px
            border-bottom-left-radius: 5px

            .list
                padding: 0px
                margin: 0px
                list-style: none

                .option
                    cursor: pointer
                    padding: 10px 0px 10px 10px
                    border-radius: 5px
                    font-size: 14px
                    &:hover
                        background-color: #41b883
                        color: white
                    &.active
                        &:hover
                            background-color: #ff6a6a
</style>
