<template>
<div class="dl-recorder">
    <dl-word-recorder :word="word"/>
    <br/>
    <span class="next-button" @click="index++">now {{ this.index + 1 }} / {{ this.words.length }}, click for the next word</span>
</div>
</template>

<script>
import DlWordRecorder from './dl-word-recorder'

export default {
    props: [
        'words',
    ],
    components: {
        DlWordRecorder,
    },
    data () {
        return {
            index: 0,
        }
    },
    computed: {
        language () {
            return this.$route.params.language
        },
        hasCoursive () {
            return !!hasCoursive[this.language]
        },
        word () {
            return this.words[this.index]
        }
    }
}
</script>

<style lang="stylus" scoped>
.dl-recorder
    > .next-button
        cursor pointer
</style>
