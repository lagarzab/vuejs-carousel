<template>
    <div
        class='flexRow'
        :style='style'
    >
        <!-- nav left arrow -->
        <side-button
            sideButtonType="before"
            @navigation='val => rotate(val)'
        ></side-button>

        <!-- main content -->
        <div class='flexColumn flexContentContainer'>
            <!-- main content container -->
            <slot
                name='content'
                :selectedIndex='slideIndex'
            >
                <div>
                    <q-icon
                        :name='dataSlides[0].iconName'
                        style='font-size:100px;'
                    ></q-icon>
                    {{ dataSlides[0].text }}
                </div>
            </slot>

            <!-- bottom nav -->
            <bottom-navigation
                :slideCount='slideCount'
                :selectedSlide='slideIndex'
                :errorMessage='errorMessage'
                @navToSlide='val => slideIndex = val'
            ></bottom-navigation>
        </div>

        <!-- nav right arrow -->
        <side-button
            sideButtonType="next"
            @navigation='val => rotate(val)'
        ></side-button>
    </div>
</template>

<script>
import sideButton from './sideButton'
import bottomNavigation from './bottomNavigation'

export default {
    name: 'main-carousel',
    components: {
        sideButton, bottomNavigation
    },
    data () {
        return {
            slideIndex: 0,
            errorMessage: '',
            dataSlides: this.slides,
        }
    },
    props: {
        slides: {
            type: Array,
            default: function () {
                return [
                    {
                        text: "This is default text, no markup passed to the 'content' slot.",
                        style: 'background-color: gold;',
                        iconName: 'report_problem',
                        default: true
                    },
                ]
            }
        }
    },
    computed: {
        style: function () {
            return this.dataSlides[this.slideIndex].style
        },
        slideCount () {
            return this.dataSlides.length
        }
    },
    methods: {
        rotate (dir) {
            if (dir > 0) {
                this.slideIndex = this.slideIndex < this.dataSlides.length - 1 ? this.slideIndex + 1 : 0
            }
            else {
                this.slideIndex = this.slideIndex == 0 ? this.dataSlides.length - 1 : this.slideIndex - 1
            }
        },
    },
    created () {
        console.log('ccc:', this.$scopedSlots)
        if (this.slides[0].default) {
            this.errorMessage = "It appears that you haven't passed the :slides props to this component!"
            return
        }
        if (!this.$scopedSlots || !this.$scopedSlots['content'] || this.$scopedSlots['content'].length == 0) {
            console.log('missing slots', 'slots:', this.$scopedSlots)
            return
        }
        this.dataSlides = this.slides
        return
    },
    created () {
        console.log('ccm:', this.$scopedSlots)
    }
}
</script>

<style lang="scss" scoped>
    .flexRow {
        display: flex;
        flex-direction: row;
    }
    .flexColumn {
        display: flex;
        flex-direction: column;
    }
    .flexContentContainer {
        flex-grow: 1;
        width: 86vw;
        align-content: center;
        align-items: center;
    }
    .flexContentContainer div:first-of-type {
        @extend .flexColumn;
        flex-grow: 1;
        text-align: center;
        align-items: center;
        justify-content: center;
    }
</style>
