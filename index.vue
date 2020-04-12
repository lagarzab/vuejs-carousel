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
            <div class='flexContent'>
                <!-- main content container -->
                <slot
                    name='default'
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
            </div>

            <!-- bottom nav -->
            <bottom-navigation
                :slideCount='slideCount'
                :selectedSlide='slideIndex'
                :errorMessage='errorMessage'
                @navToSlide='val => direct(val)'
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
import sideButton from './components/sideButton'
import bottomNavigation from './components/bottomNavigation'

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
        direct (slide) {
            this.$emit('switch', this.slideIndex - slide >= 0 ? -1 : 1 )
            this.slideIndex = slide
        },
        rotate (dir) {
            if (dir > 0) {
                this.slideIndex = this.slideIndex < this.dataSlides.length - 1 ? this.slideIndex + 1 : 0
                this.$emit('switch', 1)
            }
            else {
                this.slideIndex = this.slideIndex == 0 ? this.dataSlides.length - 1 : this.slideIndex - 1
                this.$emit('switch', -1)
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
.flexContent {
    @extend .flexRow;
    flex-grow: 1;
    text-align: center;
    align-items: center;
    justify-content: center;
}
.flexContent div:first-of-type {
    @extend .flexColumn;
    flex-grow: 1;
    text-align: center;
    align-items: center;
    justify-content: center;
}
</style>
