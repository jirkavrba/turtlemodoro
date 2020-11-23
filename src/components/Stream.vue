<template>
    <div>
        <img :src="streamUrl" width="640" height="480" :class="'stream ' + this.streamClass">
        <div class="stream__control">
            <input id="enlarge_stream" v-model="enlargeDuringPauses" type="checkbox">
            <label for="enlarge_stream">Enlarge stream during pauses</label>
        </div>
    </div>
</template>
<script>
export default {
    name: "Stream",
    props: ["phase"],
    data: () => ({
        streamUrl: "",
        enlargeDuringPauses: false,
    }),
    methods: {
        synchronizeImage: function () {
            // This is just a nasty trick to prevent self assign errors
            this.streamUrl = this.streamUrl.replace("", "");
        },
    },
    computed: {
        streamClass() {
            if (this.phase != "pomodoro" && this.enlargeDuringPauses) {
                return 'stream--enlarged';
            }

            return '';
        }
    },
    mounted: async function () {
        this.streamUrl = await fetch("https://zelva.vrba.dev/api.php")
                                .then(response => response.json())
                                .then(response => response.stream_url);

        // Sync the image every 10 seconds as it slowly falls behind sometimes
        setInterval(this.synchronizeImage, 10 * 1000);
    }
}
</script>