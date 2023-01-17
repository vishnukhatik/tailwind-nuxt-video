<template>
    <div class="h-full w-full" @keypress="videoControls($event)">
      <div class="h-full w-full"
        @mouseenter="mouseEnterVideo"
        @mouseleave="mouseLeaveVideo">
        <div
          class="relative h-full w-full bg-black"
          @click="videoControls($event)"
          :class="{ 'cursor-none': !state.contrlShow }"
        >
          <video
            id="ktv-player"
            ref="videoPlayer"
            class="ml-[10px] h-full w-full align-bottom"
            autoplay
            @timeupdate="updateTime"
            @click="playPauseToggle()"
            @keydown.right="videoControls($event)"
            @keydown.left="videoControls($event)"
          ></video>
          <div
            class="z-2147483647 absolute left-0 bottom-0 flex h-8 w-full bg-[#00000069]"
            transition="fade"
            v-show="state.contrlShow"
          >
            <!-- video play/push control start -->
            <button class="kidoo-contrl-play-btn" @click="playPauseToggle()">
              <svg
                v-if="!this.player.paused"
                class="kidoo-contrl-play-btn-icon"
                viewBox="0 0 47 57"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <defs></defs>
                <g
                  id="Page-1"
                  stroke="none"
                  stroke-width="1"
                  fill="none"
                  fill-rule="evenodd"
                >
                  <polygon
                    id="Triangle-1"
                    stroke="#FFFFFF"
                    fill="#FFFFFF"
                    points="1 56 1 1 47 28.5"
                  ></polygon>
                </g>
              </svg>
              <svg
                v-else
                class="kidoo-contrl-play-btn-icon"
                viewBox="0 0 15 22"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              > 
                <defs>
                  <path
                    id="path-1"
                    d="M0,0.979149244 L5,0.979149244 L5,22 L0,22 L0,0.979149244 Z M10,0.979149244 L15,0.979149244 L15,22 L10,22 L10,0.979149244 Z"
                  ></path>
                  <mask
                    id="mask-2"
                    maskContentUnits="userSpaceOnUse"
                    maskUnits="objectBoundingBox"
                    x="0"
                    y="0"
                    width="15"
                    height="21.0208508"
                    fill="white"
                  >
                    <use xlink:href="#path-1"></use>
                  </mask>
                </defs>
                <g
                  id="Page-1"
                  stroke="none"
                  stroke-width="1"
                  fill="none"
                  fill-rule="evenodd"
                >
                  <use
                    id="Combined-Shape"
                    stroke="#FFFFFF"
                    mask="url(#mask-2)"
                    stroke-width="2"
                    fill="#FFFFFF"
                    xlink:href="#path-1"
                  ></use>
                </g>
              </svg>
            </button>
            <!-- video play/push control end -->
            <!-- video time start -->
            <span class="kidoo-contrl-video-time-text">{{ currentVideoTime  }}</span>
            <!-- video time end -->
            <!-- video playing control start -->
            <div class="kidoo-contrl-video-slider">
              <div class="kidoo-contrl-video-inner"></div>
              <div class="kidoo-contrl-video-rail bg-white">
                <div class="kidoo-contrl-video-rail-inner"></div>
              </div>
              <div class="kidoo-contrl-video-rail">
                <div class="kidoo-contrl-video-rail-inner"></div>
              </div>
            </div>
            <!-- video playing control end -->
            <!-- video time start -->
            <span class="kidoo-contrl-video-time-text">{{ videoDuration }}</span>
            <!-- video time end -->
            <!-- video voice control start -->
            <div class="kidoo-contrl-vol-box">
              <button class="kidoo-contrl-play-btn" @click.prevent="mute()">
                <svg
                  class="kidoo-contrl-vol-btn-icon"
                  viewBox="0 0 41 44"
                  version="1.1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                >
                  <defs>
                    <path
                      id="kidoo-vol"
                      d="M8.61522369,12 L20,0.615223689 L20,37.3847763 L8.61522369,26 L1.99201702,26 C0.891856397,26 0,25.1029399 0,23.9941413 L0,14.0058587 C0,12.8980535 0.900176167,12 1.99201702,12 L8.61522369,12 L8.61522369,12 Z"
                    ></path>
                  </defs>
                  <g
                    id="Page-1"
                    stroke="none"
                    stroke-width="2"
                    fill="none"
                    fill-rule="evenodd"
                  >
                    <g id="vol" transform="translate(2.000000, 3.000000)">
                      <g id="kidoo-vol-icon">
                        <g id="Combined-Shape-Clipped">
                          <path
                            id="vol-range-2"
                            d="M25,29.5538997 C28.4589093,27.6757536 31.2629093,23.2984641 31.2629093,19.7769499 C31.2629093,16.2554357 28.4589093,11.8781461 25,10"
                            stroke="#FFFFFF"
                          ></path>
                          <path
                            id="vol-range-2"
                            d="M28,35.5538997 C33.5816016,32.5231573 38.1063837,25.4595762 38.1063837,19.7769499 C38.1063837,14.0943235 33.5816016,7.03074247 28,4"
                            stroke="#FFFFFF"
                          ></path>
                          <mask id="mask-2" fill="white">
                            <use xlink:href="#kidoo-vol"></use>
                          </mask>
                          <use
                            id="vol-path"
                            stroke="#FFFFFF"
                            stroke-width="3"
                            xlink:href="#kidoo-vol"
                          ></use>
                          <g
                            id="Combined-Shape"
                            mask="url(#mask-2)"
                            stroke="#FFFFFF"
                            stroke-width="2"
                            fill="#FFFFFF"
                          >
                            <path
                              id="kidoo-vol"
                              d="M8.61522369,12 L20,0.615223689 L20,37.3847763 L8.61522369,26 L1.99201702,26 C0.891856397,26 0,25.1029399 0,23.9941413 L0,14.0058587 C0,12.8980535 0.900176167,12 1.99201702,12 L8.61522369,12 L8.61522369,12 Z"
                            ></path>
                          </g>
                        </g>
                      </g>
                    </g>
                  </g>
                </svg>
              </button>
              <div class="kidoo-contrl-vol-slider">
                <div class="kidoo-contrl-vol-inner"></div>
                <div class="kidoo-contrl-vol-rail"></div>
              </div>
            </div>
            <!-- video voice control end -->
            <!-- video setting control start -->
            <button class="kidoo-contrl-play-btn">
              <svg
                class="kidoo-contrl-vol-btn-icon"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 50 50"
              >
                <path
                  fill="#fff"
                  d="M 22.205078 2 A 1.0001 1.0001 0 0 0 21.21875 2.8378906 L 20.246094 8.7929688 C 19.076509 9.1331971 17.961243 9.5922728 16.910156 10.164062 L 11.996094 6.6542969 A 1.0001 1.0001 0 0 0 10.708984 6.7597656 L 6.8183594 10.646484 A 1.0001 1.0001 0 0 0 6.7070312 11.927734 L 10.164062 16.873047 C 9.583454 17.930271 9.1142098 19.051824 8.765625 20.232422 L 2.8359375 21.21875 A 1.0001 1.0001 0 0 0 2.0019531 22.205078 L 2.0019531 27.705078 A 1.0001 1.0001 0 0 0 2.8261719 28.691406 L 8.7597656 29.742188 C 9.1064607 30.920739 9.5727226 32.043065 10.154297 33.101562 L 6.6542969 37.998047 A 1.0001 1.0001 0 0 0 6.7597656 39.285156 L 10.648438 43.175781 A 1.0001 1.0001 0 0 0 11.927734 43.289062 L 16.882812 39.820312 C 17.936999 40.39548 19.054994 40.857928 20.228516 41.201172 L 21.21875 47.164062 A 1.0001 1.0001 0 0 0 22.205078 48 L 27.705078 48 A 1.0001 1.0001 0 0 0 28.691406 47.173828 L 29.751953 41.1875 C 30.920633 40.838997 32.033372 40.369697 33.082031 39.791016 L 38.070312 43.291016 A 1.0001 1.0001 0 0 0 39.351562 43.179688 L 43.240234 39.287109 A 1.0001 1.0001 0 0 0 43.34375 37.996094 L 39.787109 33.058594 C 40.355783 32.014958 40.813915 30.908875 41.154297 29.748047 L 47.171875 28.693359 A 1.0001 1.0001 0 0 0 47.998047 27.707031 L 47.998047 22.207031 A 1.0001 1.0001 0 0 0 47.160156 21.220703 L 41.152344 20.238281 C 40.80968 19.078827 40.350281 17.974723 39.78125 16.931641 L 43.289062 11.933594 A 1.0001 1.0001 0 0 0 43.177734 10.652344 L 39.287109 6.7636719 A 1.0001 1.0001 0 0 0 37.996094 6.6601562 L 33.072266 10.201172 C 32.023186 9.6248101 30.909713 9.1579916 29.738281 8.8125 L 28.691406 2.828125 A 1.0001 1.0001 0 0 0 27.705078 2 L 22.205078 2 z M 23.056641 4 L 26.865234 4 L 27.861328 9.6855469 A 1.0001 1.0001 0 0 0 28.603516 10.484375 C 30.066026 10.848832 31.439607 11.426549 32.693359 12.185547 A 1.0001 1.0001 0 0 0 33.794922 12.142578 L 38.474609 8.7792969 L 41.167969 11.472656 L 37.835938 16.220703 A 1.0001 1.0001 0 0 0 37.796875 17.310547 C 38.548366 18.561471 39.118333 19.926379 39.482422 21.380859 A 1.0001 1.0001 0 0 0 40.291016 22.125 L 45.998047 23.058594 L 45.998047 26.867188 L 40.279297 27.871094 A 1.0001 1.0001 0 0 0 39.482422 28.617188 C 39.122545 30.069817 38.552234 31.434687 37.800781 32.685547 A 1.0001 1.0001 0 0 0 37.845703 33.785156 L 41.224609 38.474609 L 38.53125 41.169922 L 33.791016 37.84375 A 1.0001 1.0001 0 0 0 32.697266 37.808594 C 31.44975 38.567585 30.074755 39.148028 28.617188 39.517578 A 1.0001 1.0001 0 0 0 27.876953 40.3125 L 26.867188 46 L 23.052734 46 L 22.111328 40.337891 A 1.0001 1.0001 0 0 0 21.365234 39.53125 C 19.90185 39.170557 18.522094 38.59371 17.259766 37.835938 A 1.0001 1.0001 0 0 0 16.171875 37.875 L 11.46875 41.169922 L 8.7734375 38.470703 L 12.097656 33.824219 A 1.0001 1.0001 0 0 0 12.138672 32.724609 C 11.372652 31.458855 10.793319 30.079213 10.427734 28.609375 A 1.0001 1.0001 0 0 0 9.6328125 27.867188 L 4.0019531 26.867188 L 4.0019531 23.052734 L 9.6289062 22.117188 A 1.0001 1.0001 0 0 0 10.435547 21.373047 C 10.804273 19.898143 11.383325 18.518729 12.146484 17.255859 A 1.0001 1.0001 0 0 0 12.111328 16.164062 L 8.8261719 11.46875 L 11.523438 8.7734375 L 16.185547 12.105469 A 1.0001 1.0001 0 0 0 17.28125 12.148438 C 18.536908 11.394293 19.919867 10.822081 21.384766 10.462891 A 1.0001 1.0001 0 0 0 22.132812 9.6523438 L 23.056641 4 z M 25 17 C 20.593567 17 17 20.593567 17 25 C 17 29.406433 20.593567 33 25 33 C 29.406433 33 33 29.406433 33 25 C 33 20.593567 29.406433 17 25 17 z M 25 19 C 28.325553 19 31 21.674447 31 25 C 31 28.325553 28.325553 31 25 31 C 21.674447 31 19 28.325553 19 25 C 19 21.674447 21.674447 19 25 19 z"
                />
              </svg>
            </button>
            <!-- video setting control start -->
            <!-- video full screen control start -->
            <button class="kidoo-contrl-play-btn">
              <svg
                class="kidoo-contrl-vol-btn-icon"
                viewBox="0 0 33 33"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
              >
                <defs></defs>
                <g
                  id="Page-1"
                  stroke="none"
                  stroke-width="1"
                  fill="none"
                  fill-rule="evenodd"
                >
                  <path
                    id="Combined-Shape"
                    d="M31.1682064,22 L31.1682064,31.0073537 L22,31.0073537 M22,1 L31.0073537,1 L31.0073537,10.1682064 M1,10.0073537 L1,1 L10.1682064,1 M10.0073537,31.1682064 L1,31.1682064 L1,22"
                    stroke="#FFFFFF"
                    stroke-width="2"
                  ></path>
                </g>
              </svg>
            </button>
            <!-- video full screen control end -->
            <div
              class="kidoo-contrl-setting absolute bg-[#000000b8] px-2 py-2"
              transition="fade"
            >
              <div class="flex justify-between px-2 py-2 text-sm text-white">
                <span>Playback Speed</span>
                <span>Normal ></span>
              </div>
              <div class="flex justify-between px-2 py-2 text-sm text-white">
                <span>Quality</span>
                <span>720p ></span>
              </div>
            </div>
          </div>
          <!-- <div class="mt-[20px] ml-[10px]">
            <div class="uppercase">Current Time: {{ currentVideoTime }}</div>
            <div class="uppercase">Total Time: {{ videoDuration }}</div>
          </div>
  
          <div class="mt-6">
            <a class="custom-btn primary-btn" @click.prevent="playPauseToggle()">
              Play/Pause
            </a>
          </div> -->
        </div>
        <div class="w-1/2 pl-[40px]">
          <div>
            <a
              class="custom-btn secondary-btn block-imp"
              @click.prevent="jumpToNextEpisode()"
            >
              Next Up
            </a>
            <a
              class="custom-btn primary-btn block-imp mt-[30px]"
              @click.prevent="playPauseToggle()"
            >
              Favorite
            </a>
          </div>
        </div>
      </div>
  
      <div class="flex">
        <div class="w-1/2">
          <div class="flex">
            <div class="flex-none">
              <a class="custom-btn main-button bg-black"> Video Quality </a>
            </div>
            <div class="flex-1">
              <a class="custom-btn secondary-btn" @click.prevent="forward()">
                +10Secs
              </a>
              <a class="custom-btn secondary-btn" @click.prevent="backward()">
                -10Secs
              </a>
              <a
                class="custom-btn primary-btn"
                @click.prevent="pictureInPictureMode()"
              >
                Picture in Picture</a
              >
              <a class="custom-btn primary-btn" @click.prevent="fullScreenMode()">
                Full Screen
              </a>
            </div>
          </div>
        </div>
      </div>
  
      <div class="flex">
        <div class="flex-none">
          <a class="custom-btn main-button bg-black" @click.prevent="volUp()">
            Vol Up
          </a>
        </div>
        <div class="flex-1">
          <a class="custom-btn main-button bg-black" @click.prevent="volDown()">
            Vol Down
          </a>
          <a class="custom-btn primary-btn" @click.prevent="mute()"> Mute</a>
          <a class="custom-btn primary-btn" @click.prevent="toggleSubTitles()">
            CC/Subtitles
          </a>
        </div>
      </div>
      <div class="flex">
        <div class="w-1/2">
          <div class="flex">
            <div class="flex-none">
              <a class="custom-btn main-button bg-black"> Playback Speed </a>
            </div>
            <div class="flex-1">
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('0.25')"
              >
                0.25
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('0.5')"
              >
                0.5
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('0.75')"
              >
                0.75
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('1.0')"
              >
                Normal
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('1.25')"
              >
                1.25
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('1.5')"
              >
                1.5
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changePlaybackSpeed('1.75')"
              >
                1.75
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="flex">
        <div class="w-1/2">
          <div class="flex">
            <div class="flex-none">
              <a class="custom-btn main-button bg-black"> Video Quality </a>
            </div>
            <div class="flex-1">
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('1080')"
              >
                HD
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('720')"
              >
                3GP</a
              >
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('480')"
              >
                480P
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('360')"
              >
                360P
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('240')"
              >
                240P
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('240')"
              >
                144P
              </a>
              <a
                class="custom-btn secondary-btn"
                @click.prevent="changeQuality('auto')"
              >
                Auto
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import Hls from 'hls.js';
  
  export default {
    props: {
      seriesTitle: {
        type: String,
        default: () => {},
      },
      episodeData: {
        type: Object,
        default: () => {},
      },
      currentEpisode: {
        type: Object,
        default: () => {},
      },
      streamData: {
        type: Object,
        default: () => {},
      },
    },
    data: () => ({
      overlayData: {
        seriesTitle: '',
        episodeTitle: '',
        seasonAndEpisode: '',
        shortSummary: '',
      },
      isVideoPlaying: false,
      overlayActive: false,
      episodeLength: null,
      upNextEpisode: { id: -1 },
      player: null,
      currentVideoTime: '00:00',
      timeOut: null,
      upNextBadge: {
        visible: false,
        image: '/icon-search.svg',
        series: '',
        episode: '',
        seasonAndEpisode: '',
        text: 'Up Next...',
      },
      options: {
        autoplay: true,
        controls: true,
        preload: 'auto',
        controlBar: {
          fullscreenToggle: false,
          nativeTextTracks: true,
        },
      },
      adData: {},
      beaconData: {},
      adMarkers: [],
      hls: Object,
      videoDuration: '',
      tmp: {
        contrlHideTimer: null,
      },
      volume: {
        $volBox: null,
        muted: false,
        percent: 60,
        moving: false,
        pos: {
          start: 0,
          width: 0,
          innerWidth: 0,
          current: 0,
        },
      },
      state: {
        contrlShow: true,
        vol: 0.5,
        currentTime: 0,
        fullScreen: false,
        playing: true,
        playBackRate: 1.0,
      },
      settingMenu: false,
      playbackSetting: false,
      playBackSpeed: [
        {
          title: '0.5x',
          value: 0.5
        },
        {
          title: '0.75x',
          value: 0.75
        },
        {
          title: '1.0x',
          value: 1.0
        },
        {
          title: '1.25x',
          value: 1.25
        },
        {
          title: '1.5x',
          value: 1.5
        },
        {
          title: '1.75x',
          value: 1.75
        },
      ]
    }),
    computed: {},
    // watch: {
    //   player.currentTime: function (newVal) {
    //     this.currentVideoTime = newVal;
    //     console.log('Helloooooo' + newVal);
    //   },
    // },
    mounted() {
      this.loadVideoByHls();
    },
    beforeDestroy() {
      if (this.player) {
        this.player.removeAttribute('src');
        // this.player.dispose();
      }
    },
    methods: {
    mouseEnterVideo() {
      if (this.tmp.contrlHideTimer) {
        clearTimeout(this.tmp.contrlHideTimer);
        this.tmp.contrlHideTimer = null;
      }
      this.state.contrlShow = true;
    },
    mouseLeaveVideo(e) {
      if (this.tmp.contrlHideTimer) {
        clearTimeout(this.tmp.contrlHideTimer);
      }
      this.tmp.contrlHideTimer = setTimeout(() => {
        this.state.contrlShow = false;
        this.tmp.contrlHideTimer = null;
      }, 2000);
    },
      videoControls(e) {
        e = e || window.event;
        console.log('KeyCode', window.event);
  
        if (e.keyCode === '38') {
          // up arrow
          this.volUp();
        } else if (e.keyCode === '40') {
          // down arrow
          this.volDown();
        } else if (e.keyCode === '37') {
          // left arrow
          this.backward();
        } else if (e.keyCode === '39') {
          // right arrow
          this.forward();
        }
      },
  
      format(s) {
        let m = Math.floor(s / 60);
        m = m >= 10 ? m : '0' + m;
        s = Math.floor(s % 60);
        s = s >= 10 ? s : '0' + s;
        return m + ':' + s;
      },
      updateTime() {
        this.currentVideoTime = this.$refs.videoPlayer.currentTime.toFixed(2);
        this.currentVideoTime = this.format(this.currentVideoTime);
      },
      async loadVideoByHls() {
        this.player = this.$refs.videoPlayer;
  
        const getStreamData = await this.$getContentApi.getVideoStream(
          this.currentEpisode.videoUrl
        );
        this.source =
          getStreamData.manifestUrl !== undefined
            ? getStreamData.manifestUrl
            : getStreamData;
  
        if (Hls.isSupported()) {
          this.hls = new Hls();
          this.hls.loadSource(this.source);
          this.hls.attachMedia(this.player);
          this.player.play();
          // this.videoDuration = this.getVideoDuration();
          // this.currentVideoTime = this.player.currentTime;
          // this.videoDuration = this.player.duration;
        }
        setTimeout(() => {
          let min = Math.floor(this.player.duration / 60);
          min = min >= 10 ? min : '0' + min;
          this.videoDuration = Math.floor(this.player.duration % 60);
          this.videoDuration =
            this.videoDuration >= 10
              ? this.videoDuration
              : '0' + this.videoDuration;
          this.videoDuration = min + ':' + this.videoDuration;
          return this.videoDuration;
        }, 5000);
      },
      async loadVideo() {
        const getStreamData = await this.$getContentApi.getVideoStream(
          this.currentEpisode.videoUrl
        );
        this.source =
          getStreamData.manifestUrl !== undefined
            ? getStreamData.manifestUrl
            : getStreamData;
        if (getStreamData.trackingUrl !== undefined) {
          this.adData = await this.$getContentApi.getAdData(
            getStreamData.trackingUrl
          );
          const adBlockData = await this.$getContentApi.getBeaconData(
            this.adData
          );
          if (adBlockData !== null && adBlockData !== undefined) {
            this.adMarkers = await adBlockData.beaconMarkers;
            this.beaconData = await adBlockData.beaconData;
          }
        }
        this.getNextEpisode();
        this.overlayData = {
          seriesTitle: this.seriesTitle,
          episodeTitle: this.currentEpisode.title,
          seasonAndEpisode: this.currentEpisode.seasonAndEpisode,
          shortSummary: this.currentEpisode.shortSummary,
        };
  
        this.$nextTick(() => {
          setTimeout(() => {
            // this.player = videojs(this.$refs.videoPlayer, this.options, () => {
            //   this.player.src({
            //     src: this.source,
            //     type: 'application/x-mpegURL',
            //   });
            // });
            this.player.on('tap', () => {
              if (this.player.userActive() === true) {
                this.player.userActive(false);
                this.hideOverlay();
              } else {
                this.player.userActive(true);
                this.showOverlay();
              }
            });
            this.player.on('useractive', () => {
              this.showOverlay();
            });
            this.player.on('userinactive', () => {
              if (this.isVideoPlaying) {
                this.hideOverlay(1);
              }
            });
            this.player.on('pause', () => {
              this.showOverlay();
              this.isVideoPlaying = false;
            });
            this.player.on('play', () => {
              this.hideOverlay();
              this.isVideoPlaying = true;
            });
  
            // todo: On complete > show Up Next and setTimeout
            // this.player.on('ended', () => {})
  
            this.player.on('timeupdate', () => {
              this.handlePlayerTime(this.player.currentTime());
              this.currentVideoTime = this.player.currentTime();
            });
            this.player.play();
            this.currentVideoTime = this.player.duration;
            this.showOverlay(250);
  
            setTimeout(() => {
              this.player.markers({
                markerStyle: {
                  // display: 'none', // Comment out see marker dots
                },
                markerTip: {
                  display: false,
                },
                breakOverlay: {
                  display: false,
                },
                onMarkerClick: false,
                onMarkerReached: (marker) => {
                  // console.log('index', index) // add (marker, index) to the func
                  this.adsSendBeacon(marker);
                },
                markers: this.adMarkers,
              });
            }, 2000);
          }, 500); // 500 was the sweet spot
        });
      },
      async setNewAdBeacons() {
        if (this.streamData.trackingUrl) {
          this.adData = await this.$getContentApi.getAdData(
            this.streamData.trackingUrl
          );
  
          const adBlockData = await this.$getContentApi.getBeaconData(
            this.adData
          );
          if (adBlockData !== null && adBlockData !== undefined) {
            this.adMarkers = await adBlockData.beaconMarkers;
            this.beaconData = await adBlockData.beaconData;
          }
        }
  
        // console.log('this.adData', this.adData)
        this.player.markers({
          markerStyle: {
            display: 'none', // Comment out see marker dots
          },
          markerTip: {
            display: false,
          },
          breakOverlay: {
            display: false,
          },
          onMarkerClick: false,
          onMarkerReached: (marker) => {
            this.adsSendBeacon(marker);
          },
          markers: this.adMarkers,
        });
      },
      adsSendBeacon(marker) {
        if (!marker.beaconSent) {
          marker.beaconUrls.forEach((url) => {
            this.$axios
              .$get(`${url}`)
              .catch((error) => console.log('Request canceled', error));
          });
          marker.beaconSent = true;
        }
      },
      handlePlayerTime(timeInSeconds) {
        if (timeInSeconds >= this.player.duration() - 8) {
          this.showUpNextEpisode();
        } else if (this.upNextBadge.visible) this.upNextBadge.visible = false;
  
        if (timeInSeconds >= this.player.duration()) {
          setTimeout(() => {
            this.jumpToNextEpisode();
          }, 2000);
        }
      },
      showUpNextEpisode() {
        if (!this.upNextBadge.visible) this.upNextBadge.visible = true;
      },
      jumpToNextEpisode() {
        this.player.pause();
        this.player.currentTime = 0;
  
        this.$emit('episodeChange', this.getNextEpisode());
      },
      getNextEpisode() {
        const curArrayPos = this.episodeData.episodeList.findIndex((episode) => {
          return episode.id === this.currentEpisode.id;
        });
        if (
          curArrayPos === -1 ||
          curArrayPos === this.episodeData.episodeList.length - 1
        ) {
          // episode not found in array or its the last one. go to 1st episode
          this.upNextEpisode = this.episodeData.episodeList[0];
        } else {
          // episode found in array. go to next episode
          this.upNextEpisode = this.episodeData.episodeList[curArrayPos + 1];
        }
        this.upNextBadge.image = this.upNextEpisode.imageUrl;
        this.upNextBadge.episode = this.upNextEpisode.title;
        this.upNextBadge.seasonAndEpisode = this.upNextEpisode.seasonAndEpisode;
        this.upNextBadge.series = this.getSeriesTitle(
          this.upNextEpisode.seriesId
        );
        return this.upNextEpisode;
      },
      getSeriesTitle(id) {
        // todo.. find series in case its the autoplay random mode..
        if (id) return this.seriesTitle;
      },
      toggleOverlay() {
        if (this.player.userActive() === true) {
          this.player.userActive(false);
          this.hideOverlay();
        } else {
          this.player.userActive(true);
          this.showOverlay();
        }
      },
      tapOverlay() {
        if (this.isVideoPlaying === true) {
          this.showOverlay();
        }
      },
      playVideo() {
        this.player.play();
      },
      pauseVideo() {
        this.player.pause();
      },
      playPauseToggle() {
        if (!this.player.paused) {
          this.pauseVideo();
        } else {
          this.playVideo();
        }
      },
      forward() {
        this.player.currentTime += 10;
        console.log('Time', this.player.currentTime);
      },
      backward() {
        this.player.currentTime -= 10;
      },
      volUp() {
        const vol = this.player.volume;
        if (vol < 1) {
          this.player.volume = vol + 0.01;
        }
      },
      volDown() {
        const vol = this.player.volume;
        if (vol >= 0) {
          this.player.volume = vol - 0.01;
        }
      },
      mute() {
        this.player.muted = !this.player.muted;
      },
      toggleSubTitles() {
        this.hls.subtitleDisplay = !this.hls.subtitleDisplay;
      },
      changeQuality(quality) {
        switch (quality) {
          case 'auto':
            this.hls.currentLevel = -1;
            break;
          case '240':
            this.hls.currentLevel = 0;
            break;
          case '360':
            this.hls.currentLevel = 1;
            break;
          case '480':
            this.hls.currentLevel = 2;
            break;
          case '720':
            this.hls.currentLevel = 3;
            break;
          case '1080':
            this.hls.currentLevel = 4;
            break;
        }
      },
      pictureInPictureMode() {
        this.player.requestPictureInPicture();
      },
      fullScreenMode() {
        this.player.requestFullscreen();
      },
      getVideoDuration() {
        // if (this.videoPlayer) {
        //   const totalDuration = this.videoPlayer.duration;
        //   console.log(totalDuration);
        //   const totalMin = Math.floor(totalDuration / 60);
        //   let totalSec = Math.floor(totalDuration % 60);
        //   totalSec = totalSec < 10 ? '0' + totalSec : totalSec;
        //   return totalMin + ':' + totalSec;
        // } else {
        //   return '';
        // }
        return '';
      },
      changePlaybackSpeed(rate) {
        this.videoPlayer.playbackRate = parseFloat(rate);
      },
      showOverlay() {
        if (this.timeOut) clearTimeout(this.timeOut);
        this.overlayActive = true;
        this.player.userActive(true);
      },
      hideOverlay(delay = 1200) {
        if (this.timeOut) clearTimeout(this.timeOut);
        this.timeOut = setTimeout(() => {
          this.overlayActive = false;
          this.player.userActive(false);
        }, delay);
      },
      goBackToSeries() {
        this.$router.push({
          name: 'seriesId-slug',
          params: this.$router.history.current.params,
        });
      },
      buildPlaylist() {},
      getPoster(data) {
        let posterImg;
        data.forEach((element) => {
          if (element.height === 720) {
            posterImg = element.url;
          }
        });
        return posterImg;
      },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  .main-button {
    color: white !important;
  }
  .video-player {
    max-width: 75% !important;
    margin: 40px !important;
  }
  .overlayWrapper {
    transition: opacity 250ms ease-in-out 0ms;
    background-color: rgba(43, 51, 63, 0.7);
    height: calc(100vh - 54px);
    &.active {
      @apply opacity-100;
    }
  }
  .video-js .vjs-control-bar {
    background-color: rgba(43, 51, 63, 0.6);
    &.vjs-user-inactive {
      cursor: none;
    }
  }
  // Up next badge
  .upNextBadgeWrapper {
    transition: all 500ms ease-in-out 250ms;
    left: 600px;
    @apply relative;
  }
  .upNextBadgeActive {
    min-width: 420px;
    max-width: 596px;
    @apply left-5;
  }
  // MARKER CSS
  .vjs-marker {
    @apply absolute left-0 bottom-0 hidden h-full opacity-100;
    &:hover {
      cursor: pointer;
      transform: scale(1.3, 1.3);
    }
  }
  .vjs-break-overlay {
    visibility: hidden;
    position: absolute;
    z-index: 100000;
    top: 0;
    .vjs-break-overlay-text {
      padding: 9px;
      text-align: center;
    }
  }
  
  .duration {
    margin-top: -24px;
    margin-left: 40px;
  }
  
  .primary-btn {
    background: #00d7ff !important;
  }
  .block-imp {
    display: block !important;
  }
  
  .custom-btn {
    border: 1px solid black;
    color: black;
    padding: 10px 5px;
    margin: 10px;
    width: 170px;
    display: inline-block;
    text-align: center;
    cursor: pointer;
    text-transform: uppercase;
  }
  .secondary-btn {
    color: white !important;
    background: grey !important;
  }
  .kidoo-contrl-play-btn {
    position: relative;
    // height: 100%;
    background: none;
    border: none;
    height: 2rem;
    width: 4rem;
    outline: none;
    vertical-align: top;
  }
  .kidoo-contrl-play-btn-icon {
    position: absolute;
    height: 1rem;
    width: 1rem;
    top: 50%;
    left: 50%;
    margin-top: -0.5rem;
    margin-left: -0.5rem;
  }
  .kidoo-contrl-vol-btn-icon {
    position: absolute;
    height: 1.1rem;
    width: 1.1rem;
    top: 50%;
    left: 50%;
    margin-top: -0.55rem;
    margin-left: -0.55rem;
  }
  .kidoo-contrl-vol-slider {
    position: relative;
    display: inline-block;
    // height: 100%;
    width: 4rem;
    height: 2rem;
    overflow: hidden;
    transition: all 0.2s ease-in;
  }
  .kidoo-contrl-vol-rail {
    position: absolute;
    top: 50%;
    width: 4rem;
    height: 0.2rem;
    margin-top: -0.05rem;
    background: #fff;
    border-radius: 40px;
  }
  .kidoo-contrl-vol-inner {
    position: absolute;
    display: inline-block;
    left: 0;
    top: 50%;
    background: #fff;
    width: 0.6rem;
    height: 0.6rem;
    border-radius: 50%;
    margin-top: -0.25rem;
    z-index: 2;
    cursor: pointer;
    box-shadow: rgb(0 0 0 / 44%) 1px 1px 3px 2px;
  }
  .kidoo-contrl-vol-box {
    display: flex;
  }
  .kidoo-contrl-video-slider {
    position: relative;
    display: inline-block;
    height: 100%;
    width: 100%;
    overflow: hidden;
    margin: 0 0.5rem;
    transition: all 0.2s ease-in;
  }
  .kidoo-contrl-video-rail {
    position: absolute;
    top: 50%;
    width: 100%;
    height: 0.2rem;
    margin-top: -0.05rem;
    background: rgba(255, 255, 255, 0.5);
    overflow: hidden;
    border-radius: 40px;
  }
  .kidoo-contrl-video-rail-inner {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 0.2rem;
    background: rgb(255, 255, 255);
    transition: transform 0.2s;
    border-radius: 40px;
  }
  .kidoo-contrl-video-inner {
    position: absolute;
    display: inline-block;
    left: 0;
    top: 50%;
    background: #fff;
    width: 0.7rem;
    height: 0.7rem;
    border-radius: 50%;
    margin-top: -0.29rem;
    z-index: 2;
    cursor: pointer;
    transition: all 16ms;
    box-shadow: rgb(0 0 0 / 44%) 1px 1px 3px 2px;
  }
  
  /* .kidoo-contrl-video-time {
    padding: 0 1rem;
  } */
  .kidoo-contrl-video-time-text {
    color: #fff;
    line-height: 2rem;
    font-size: 0.8rem;
  }
  // ::-webkit-media-controls {
  //   display: none !important;
  // }
  // video::-webkit-media-controls {
  //   display: none !important;
  // }
  // video::-webkit-media-controls-enclosure {
  //   display: none !important;
  // }
  
  /* .kidoo-contrl-video-time {
    padding: 0 1rem;
  } */
  
  .fade-transition {
    transition: opacity 0.3s ease;
  }
  .fade-enter {
    opacity: 1;
  }
  .fade-leave {
    opacity: 0;
  }
  .kidoo-contrl-setting {
    border-radius: 5px;
    z-index: 9999;
    top: 50%;
    right: 0%;
    margin-top: -7.5rem;
    margin-right: 0.4rem;
    width: 13rem;
  }
  .kidoo-contrl-setting-playbackspeed {
    margin-top: -16rem !important;
  }
  @media all and (max-width: 768px) {
    .kidoo-contrl-vol-slider {
      width: 3rem;
    }
  
    /* .kidoo-contrl-video-time {
      padding: 0 0.2rem;
    } */
    .kidoo-contrl-vol-box .kidoo-contrl-play-btn {
      width: 2rem;
    }
  }
  </style>