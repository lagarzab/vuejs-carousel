<template>
    <div>
        <a
            href='#'
            class='arrowBtn'
            @click='$emit("navigation", nextValue)'
        >
            <div
                class='arrowNav'
                :style='"float:" + floatDirection + ";"'
            >
                <div>
                    <q-icon :name='iconName'></q-icon>
                </div>
            </div>
        </a>
    </div>
</template>

<script>
export default {
    name: 'side-button',
    data () {
        return {

        }
    },
    props: [
        'sideButtonType' //this should be passed as before or next
    ],
    computed: {
        iconName () {
            // should return navigate_before || navigate_next
            return 'navigate_' + this.sideButtonType
        },
        floatDirection () {
            return this.sideButtonType === 'before' ? 'left' : 'right'
        },
        nextValue () {
            return this.sideButtonType === 'before' ? -1 : 1
        }
    },
    rotate (val) {
        this.$emit('navigation', this.nextValue)
    }
}
</script>

<style lang="scss" scoped>

    .arrowNav {
        position: relative;
        font-size: 100px;
        height: 100vh;
        display: flex;
        flex-grow: 1;
        width: 7vw;
        align-items: center;
        justify-content: center;
        color: white;
    }
    .arrowNav:hover {
        background-color: rgba(black, 0.25);
    }

</style>
