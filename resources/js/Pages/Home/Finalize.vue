<template>
    <div>
        <div v-if="content" class="content-loading"></div>
        <div v-if="content" class="content" ref="screenshot" id="capture">
            <div class="header">
                <h1 class="title">Tastetify</h1>
                <span>{{ time_frame }} Best Menu by {{ user.name }}</span>
            </div>
            <hr class="text-white">
            <ul>
                <li v-for="(track, index) in tracks" :key="index">
                    <div class="d-flex justify-content-between">
                        <div class="track text-white">
                            {{ track.title }} <span class="artist">by {{ track.artists }}</span>
                        </div>
                        <div class="px-3 pr-0 duration text-white">
                            {{ track.duration }}
                        </div>
                    </div>
                </li>
            </ul>

            <div class="d-flex images justify-content-between mt-4">
                
                <div v-for="(image, index) in images" :key="index" v-bind:style="{ backgroundImage: 'url(' + image + ')' }"></div>
                
            </div>

            <div>
                <p class="footer">Thanks for visiting us!</p>
            </div>
        </div>

        <div class="html2canvas-container text-center mb-5">
            <div id="canvasRendered"></div>
            <a download="" href="" class="btn btn-outline-primary mt-3 download" id="download">Save as image</a>
        </div>

    </div>
</template>

<script>


import html2canvas from 'html2canvas'

export default {
    props: {
        user: Object,
        tracks: Array,
        images: Array,
        time_frame: String
    },
    data() {
        return {
            output: null,
            content: true,
            imageUrl: 'https://cdn.glitch.com/4c9ebeb9-8b9a-4adc-ad0a-238d9ae00bb5%2Fmdn_logo-only_color.svg?1535749917189', // cross-domain address
		    screenshotImage: ''
        }
    },
    methods: {
        async screenshot() {            
            const opts = {
                useCORS: true
            }
            const el = this.$refs.screenshot
            const canvas = await html2canvas(el, opts)
            
            this.screenshotImage = canvas.toDataURL('image/jpg')
            this.content = false
            document.getElementById('canvasRendered').append(canvas)
            let download_btn = document.getElementById('download')
            download_btn.setAttribute('href', canvas.toDataURL())
            download_btn.setAttribute('download', 'Tastetify.png')
        }

    },
    mounted() {
        this.screenshot()
    }
}
</script>

<style scoped>
.content-loading {
    width: 100%;
    height: 100%;
    background-color: #F1F2F3;
    position: fixed;
    z-index: 2;
    background-image: url('/img/loading.gif');
    background-repeat: no-repeat;
    background-position: center;
}

.header {
    margin-bottom: 0;
}

h1.title {
    font-size: 34px;
    margin-bottom: 0;
    color: #ffe100;
}

.header span {
    font-size: 12px;
    margin-left: 8px;
    color: #ffe100;
}

ul {
    margin: 0;
    padding: 0;
    list-style: none;
}
li {
    margin-bottom: 8px;
}
.content {
    max-width: 380px;
    padding: 1.3rem;
    background-image: url('/img/texture2.jpg');
    background-position: center center;
    background-size: cover;
    background-color: rgb(0, 0, 0);
    border:0;
}
.pr-0 {
    padding-right: 0!important;
}

.track {
    font-size: 15px;
}

.duration {
    font-size: 13.5px;
}

span.artist {
    display: block;
    font-size: 11.5px;
}

.images div {
    width: 100px;
    height: 100px;
    background-size: cover;
    border: 1px solid grey;
    border-radius: 8px;
}

p.footer {
    color: #ffe100;
    text-align: center;
    margin-top: 18px;
    margin-bottom: 0;
    font-size: 12px;
}

#canvasRendered {
    margin: 20px auto;
    text-align: center;
}

</style>