<template>
<div class="dl-recorder">
    <div class="hint">Please, record this word:</div>
    <div class="word">{{ word.original }}</div>
    <div class="translation">({{ word.translation }})</div>

    <button class="toggler" @click="toggleRecordingState"
    >{{ recordingInProgress ? 'speak now, press again when done' : 'press to start recording' }}</button>

    <div class="debug" v-show="recorded">
        your record:
        <br/>
        <audio ref="player" controls/>
        <br/>
        <br/>
        <a href="" ref="downloadLink">download</a>
    </div>
</div>
</template>

<script>
import MicRecorder from 'mic-recorder-to-mp3'

export default {
    props: [
        'word'
    ],
    data () {
        return {
            recordingInProgress: false,
            recorded: false,
            recorder: new MicRecorder({ bitRate: 128 }),
        }
    },
    methods: {
        toggleRecordingState () {
            this.recordingInProgress = !this.recordingInProgress
            if (this.recordingInProgress) {
                this.recorder.start()
            } else {
                this.recorder
                    .stop()
                    .getMp3().then(([buffer, blob]) => {
                        // do what ever you want with buffer and blob
                        // Example: Create a mp3 file and play
                        const filename = `${this.word}.mp3`
                        const file = new File(buffer, filename, {
                            type: blob.type,
                            lastModified: Date.now()
                        });

                        const url = URL.createObjectURL(file);

                        this.$refs.downloadLink.href = url
                        this.$refs.downloadLink.download = filename;
                        this.$refs.player.src = url
                        this.recorded = true

                    }).catch((e) => {
                        console.error(e);
                        this.recorded = false
                    });
            }
        },
    }
}
</script>

<style lang="stylus" scoped>
.dl-recorder
    > .hint
        //
    > .word
        font-size 4em
        margin .2em 0
    > .toggler
        margin 1em 0
        padding 1em
        border 2px solid red
        cursor pointer
        &:before
            display inline-block
            content ''
            width 1em
            height 1em
            border-radius 1em
            background red
            margin-right 1em
    > .debug
        //
</style>