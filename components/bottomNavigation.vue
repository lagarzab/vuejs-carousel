<template>
    <div class='bottomNav'>
        <div
            v-if='errorMessage'
            style='background-color:red; font-weight:bold; color:white;'
        >{{ errorMessage }}</div>
        <template v-for='i in slideCount'>
            <button
                :key='i-1'
                class='navDot'
                @click='$emit("navToSlide", i-1)'
                :style='i-1 == selectedSlide ? greyDot : ""'
            >
                <q-icon name='lens'></q-icon>
            </button>
        </template>
    </div>
</template>

<script>
    export default {
        props: [
            'slideCount', 'selectedSlide', 'errorMessage'
        ],
        data () {
            return {
                greyDot: 'color: rgba(0, 0, 0, 0.5);'
            }
        }
    }
</script>

<style lang="scss" scoped>
    .bottomNav {
        text-align: center;
        width: 100%;
        align-self: flex-end;
    }
    .navDot {
        border: none;
        background-color: transparent;
        color: white;
        text-decoration: none;
        margin: 3px;
    }
</style>
