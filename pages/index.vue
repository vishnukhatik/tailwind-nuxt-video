<template>
  <div>
    <div>
      <video-player :sources="video.sources" :options="video.options"></video-player>
    </div>
  </div>
</template>

<script>
import VideoPlayer from '../components/VideoPlayer.vue'
export default {
  components: { VideoPlayer },
  name: 'IndexPage',
  data() {
    return {
      video: {
                sources: [{
                    src: 'https://storage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4',
                    type: 'video/mp4'
                }],
                options: {
                    autoplay: true,
                    volume: 0.6,
                    poster: 'https://storage.googleapis.com/gtv-videos-bucket/sample/images/BigBuckBunny.jpg'
                }
        }
    }
  }
}
</script>
