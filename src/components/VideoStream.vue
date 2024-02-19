<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 my-3">
                <h2>Xona</h2>
                <input v-model="roomId">
            </div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <div class="">
                    <vue-webrtc ref="webrtc" width="100%" :roomId="roomId" :enableLogs="true" v-on:joined-room="logEvent"
                        v-on:left-room="logEvent" v-on:opened-room="logEvent" v-on:share-started="logEvent"
                        v-on:share-stopped="logEvent" @error="onError" socketURL="https://weston-vue-webrtc-lobby.azurewebsites.net" />
                </div>
                <div class="row">
                    <div class="col-md-12 my-3">
                        <button id="join" type="button" class="btn btn-primary" @click="onJoin">Qo'shilish</button>
                        <button id="leave" type="button" class="btn btn-primary d-none" @click="onLeave">Tark etish</button>
                        <button type="button" class="btn btn-primary" @click="onCapture">Rasmga olish</button>
                        <button type="button" class="btn btn-primary" @click="onShareScreen">Ekran ulashish</button>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <h2>Olingan rasm</h2>
                <figure class="figure">
                    <img :src="img" class="img-responsive" alt="Responsive image" />
                </figure>
            </div>
        </div>
    </div>
</template>

<script>
import { VueWebRTC } from 'vue-webrtc';

export default {
    name: 'VideoStream',
    components: {
        'vue-webrtc': VueWebRTC
    },
    data() {
        return {
            img: null,
            roomId: "public-room-v3"
        };
    },
    mounted: function () {
    },
    computed: {
    },
    watch: {
    },
    methods: {
        onCapture() {
            this.img = this.$refs.webrtc.capture();
        },
        onJoin() {
            this.$refs.webrtc.join();
            document.getElementById('join').classList.add('d-none');
            document.getElementById('leave').classList.remove('d-none');
        },
        onLeave() {
            this.$refs.webrtc.leave();
            document.getElementById('join').classList.remove('d-none');
            document.getElementById('leave').classList.add('d-none');
        },
        onShareScreen() {
            this.img = this.$refs.webrtc.shareScreen();
        },
        onError(error, stream) {
            console.log('On Error Event', error, stream);
        },
        logEvent(event) {
            console.log('Event : ', event);
        },
    }
};
</script>

<style>
.btn {
    margin-right: 8px;
}
</style>