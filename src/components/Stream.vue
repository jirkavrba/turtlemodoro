<template>
    <img :src="streamUrl" width="100" height="100">
</template>
<script>
export default {
    name: "Stream",
    data: () => ({
        streamUrl: ""
    }),
    methods: {
        synchronizeImage: function () {
            console.log("Synchronizing stream image");

            // This is just a nasty trick to prevent self assign errors
            this.streamUrl = this.streamUrl.replace("", "");
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