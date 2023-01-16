<template>
  <div id="app">
    <div class="flex">
      <div class="w-1/2">
        <div
          class="kidoo-video-container"
          @mouseenter="mouseEnterVideo"
          @mouseleave="mouseLeaveVideo"
        >
          <video
            :class="{ 'cursor-none': !state.contrlShow }"
            class="align-bottom w-full h-full"
            :poster="options.poster"
          >
            <source
              v-for="source in sources"
              :src="source.src"
              :type="source.type"
            />
          </video>
          <div
            class="absolute flex left-0 bottom-0 h-8 w-full z-2147483647 bg-[#00000069]"
            transition="fade"
            v-show="state.contrlShow"
          >
            <!-- video play/push control start -->
            <button class="kidoo-contrl-play-btn" @click="play">
              <svg
                class="kidoo-contrl-play-btn-icon"
                v-if="!state.playing"
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
                class="kidoo-contrl-play-btn-icon"
                v-else
                viewBox="0 0 15 22"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <defs>
                  <path
                    d="M0,0.979149244 L5,0.979149244 L5,22 L0,22 L0,0.979149244 Z M10,0.979149244 L15,0.979149244 L15,22 L10,22 L10,0.979149244 Z"
                    id="path-1"
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
              <span class="kidoo-contrl-video-time-text">{{
                video.currentTime
              }}</span>
            <!-- video time end -->
            <!-- video playing control start -->
            <div
              class="kidoo-contrl-video-slider"
              @click="slideClick"
              @mousedown="videoMove"
            >
              <div
                class="kidoo-contrl-video-inner"
                :style="{
                  transform: `translate3d(${video.pos.current}px, 0, 0)`,
                }"
              ></div>
              <div class="kidoo-contrl-video-rail bg-white">
                <div
                  class="kidoo-contrl-video-rail-inner"
                  :style="{
                    width: `${video.pos.current}px`,
                  }"
                ></div>
              </div>
              <div class="kidoo-contrl-video-rail">
                <div
                  class="kidoo-contrl-video-rail-inner"
                  :style="{
                    transform: 'translate3d(' + video.loaded + '%, 0, 0)',
                  }"
                ></div>
              </div>
            </div>
            <!-- video playing control end -->
            <!-- video time start -->
              <span class="kidoo-contrl-video-time-text">{{
                video.displayTime
              }}</span>
            <!-- video time end -->
            <!-- video voice control start -->
            <div class="kidoo-contrl-vol-box">
              <button class="kidoo-contrl-play-btn" @click="volMuted">
                <svg
                  class="kidoo-contrl-vol-btn-icon"
                  viewBox="0 0 41 44"
                  version="1.1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                >
                  <defs>
                    <path
                      d="M8.61522369,12 L20,0.615223689 L20,37.3847763 L8.61522369,26 L1.99201702,26 C0.891856397,26 0,25.1029399 0,23.9941413 L0,14.0058587 C0,12.8980535 0.900176167,12 1.99201702,12 L8.61522369,12 L8.61522369,12 Z"
                      id="kidoo-vol"
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
                            v-show="volume.percent > 1 && !volume.muted"
                            d="M25,29.5538997 C28.4589093,27.6757536 31.2629093,23.2984641 31.2629093,19.7769499 C31.2629093,16.2554357 28.4589093,11.8781461 25,10"
                            id="vol-range-2"
                            stroke="#FFFFFF"
                          ></path>
                          <path
                            v-show="volume.percent > 70 && !volume.muted"
                            d="M28,35.5538997 C33.5816016,32.5231573 38.1063837,25.4595762 38.1063837,19.7769499 C38.1063837,14.0943235 33.5816016,7.03074247 28,4"
                            id="vol-range-2"
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
                              d="M8.61522369,12 L20,0.615223689 L20,37.3847763 L8.61522369,26 L1.99201702,26 C0.891856397,26 0,25.1029399 0,23.9941413 L0,14.0058587 C0,12.8980535 0.900176167,12 1.99201702,12 L8.61522369,12 L8.61522369,12 Z"
                              id="kidoo-vol"
                            ></path>
                          </g>
                        </g>
                      </g>
                    </g>
                  </g>
                </svg>
              </button>
              <div
                class="kidoo-contrl-vol-slider"
                @click="volSlideClick"
                @mousedown="volMove"
              >
                <div
                  class="kidoo-contrl-vol-inner"
                  :style="{
                    transform: `translate3d(${volume.pos.current}px, 0, 0)`,
                  }"
                ></div>
                <div class="kidoo-contrl-vol-rail"></div>
              </div>
            </div>
            <!-- video voice control end -->
            <!-- video setting control start -->
            <button class="kidoo-contrl-play-btn" @click="toggleSettingIcon()">
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
            <button class="kidoo-contrl-play-btn" @click="fullScreen">
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
                    d="M31.1682064,22 L31.1682064,31.0073537 L22,31.0073537 M22,1 L31.0073537,1 L31.0073537,10.1682064 M1,10.0073537 L1,1 L10.1682064,1 M10.0073537,31.1682064 L1,31.1682064 L1,22"
                    id="Combined-Shape"
                    stroke="#FFFFFF"
                    stroke-width="2"
                  ></path>
                </g>
              </svg>
            </button>
            <!-- video full screen control end -->
            <div class="kidoo-contrl-setting absolute px-2 py-2 bg-[#000000b8]"  transition="fade" v-show="settingMenu">
              <div class="flex justify-between px-2 py-2 text-sm text-white hover:bg-[#624b4bb8] cursor-pointer rounded" @click="playbackSetting = !playbackSetting; settingMenu = false">
                <span class="font-bold text-white">Playback Speed</span>
                <span>{{ currentPlaybackRate }}</span>
              </div>
              <div class="flex justify-between px-2 py-2 text-sm text-white hover:bg-[#624b4bb8] cursor-pointer rounded">
                <span class="font-bold text-white">Quality</span>
                <span>720p</span>
              </div>
            </div>
            <div class="kidoo-contrl-setting kidoo-contrl-setting-playbackspeed absolute px-2 py-2 bg-[#000000b8]"  transition="fade" v-show="playbackSetting">
              <div class="h-52 overflow-auto">
              <div v-for="playback in playBackSpeed" :key="playback.value" @click="changePlaybackSpeed(playback.value)" :class="activePlaySpeedRate(playback.value)" class="flex justify-between px-2 py-2 text-sm text-white hover:bg-[#624b4bb8] cursor-pointer rounded">
                <span>{{  playback.title  }}</span>
              </div>
            </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
const getMousePosition = function (e, type = "x") {
  if (type === "x") {
    return e.pageX;
  }
  return e.pageY;
};
const pad = (val) => {
  val = Math.floor(val);
  if (val < 10) {
    return "0" + val;
  }
  return val + "";
};
const timeParse = (sec) => {
  let min = 0;
  min = Math.floor(sec / 60);
  sec = sec - min * 60;
  return pad(min) + ":" + pad(sec);
};
export default {
  props: {
    sources: Array,
    options: {
      type: Object,
      default() {
        return {
          autoplay: false,
          volume: 0.9,
          poster: "",
        };
      },
    },
  },
  data() {
    return {
      videoData: null,
      video: {
        videoDataSlider: null,
        len: 0,
        current: 0,
        loaded: 0,
        moving: false,
        displayTime: "00:00",
        currentTime: "00:00",
        pos: {
          start: 0,
          width: 0,
          innerWidth: 0,
          current: 0,
        },
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
      player: {
        $player: null,
        pos: null,
      },
      tmp: {
        contrlHideTimer: null,
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
    };
  },
  computed: {
    currentPlaybackRate() {
      return this.playBackSpeed.find((speed) => speed.value === this.state.playBackRate).title
    }
  },
  ready() {
    this.init();
  },
  mounted() {
    this.init();
  },
  beforeDestroy() {
    document.body.removeEventListener("mousemove", this.mouseMoveAction);
    document.body.removeEventListener("mouseup", this.mouseUpAction);
  },
  methods: {
    init() {
      this.videoData = this.$el.getElementsByTagName("video")[0];
      this.initCore();
      if (this.options.autoplay) {
        this.play();
      }
      document.body.addEventListener("mousemove", this.mouseMoveAction, false);
      document.body.addEventListener("mouseup", this.mouseUpAction, false);
      document.onkeydown = (event) => {
        console.log('event.keyCode', event.keyCode);
      switch (event.keyCode) {
       case 32:
            event.preventDefault();
            this.play();
          break;
       case 39:
            event.preventDefault();
            this.videoData.currentTime += 10;
            console.log('this.videoData.currentTime', this.videoData.currentTime);
       case 37:
       event.preventDefault();
            this.videoData.currentTime -= 10;
            console.log('this.videoData.currentTime', this.videoData.currentTime);
       
    }
};
      this.videoData.playbackRate = parseFloat(1.0)
      this.state.playBackRate =  this.videoData.playbackRate
    },
    initCore() {
      this.initVol();
      this.initVideo();
      this.initPlayer();
      const vol = this.options.volume || 0.9;
      this.setVol(vol);
    },
    initPlayer() {
      const $player = this.$el.getElementsByClassName("kidoo-video-container")[0];
      this.player.pos = $player.getBoundingClientRect();
      this.player.$player = $player;
    },
    initVol() {
      const $volBox = this.$el.getElementsByClassName(
        "kidoo-contrl-vol-slider"
      )[0];
      const $volInner = $volBox.getElementsByClassName(
        "kidoo-contrl-vol-inner"
      )[0];
      this.volume.$volBox = $volBox;
      this.volume.pos.innerWidth = $volInner.getBoundingClientRect().width;
      this.volume.pos.start = $volBox.getBoundingClientRect().left;
      this.volume.pos.width =
        $volBox.getBoundingClientRect().width - this.volume.pos.innerWidth;
    },
    initVideo() {
      const videoDataSlider = this.$el.getElementsByClassName(
        "kidoo-contrl-video-slider"
      )[0];
      const videoDataInner = videoDataSlider.getElementsByClassName(
        "kidoo-contrl-video-inner"
      )[0];
      this.videoDataSlider = videoDataSlider;
      this.video.pos.start = videoDataSlider.getBoundingClientRect().left;
      this.video.pos.innerWidth = videoDataInner.getBoundingClientRect().width;
      this.video.pos.width =
        videoDataSlider.getBoundingClientRect().width - this.video.pos.innerWidth;
      this.getTime();
    },
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
    toggleContrlShow() {
      this.state.contrlShow = !this.state.contrlShow;
    },
    toggleSettingIcon() {
      this.settingMenu = !this.settingMenu;
      if(this.settingMenu) this.playbackSetting = false
    },
    getTime() {
      this.videoData.addEventListener("durationchange", (e) => {
        console.log(e);
      });
      this.videoData.addEventListener("progress", (e) => {
        this.video.loaded =
          (-1 + this.videoData.buffered.end(0) / this.videoData.duration) * 100;
      });
      this.video.len = this.videoData.duration;
    },
    setVideoByTime(percent) {
      this.videoData.currentTime = Math.floor(percent * this.video.len);
    },
    play() {
      this.state.playing = !this.state.playing;
      if (this.videoData) {
        if (this.state.playing) {
          this.videoData.play();
          this.mouseLeaveVideo();
          this.videoData.addEventListener("timeupdate", this.timeline);
          this.videoData.addEventListener("ended", (e) => {
            this.state.playing = false;
            this.video.pos.current = 0;
            this.videoData.currentTime = 0;
          });
        } else {
          this.videoData.pause();
        }
      }
    },
    timeline() {
      console.log('timeline update');
      const percent = this.videoData.currentTime / this.videoData.duration;
      this.video.pos.current = (this.video.pos.width * percent).toFixed(3);
      this.video.displayTime = timeParse(
        this.videoData.duration - this.videoData.currentTime
      );
      this.video.currentTime = timeParse(this.videoData.currentTime);
    },
    volMove(e) {
      this.initVol();
      this.volume.moving = true;
    },
    videoMove(e) {
      this.initVideo();
      this.video.moving = true;
    },
    slideClick(e) {
      this.videoSlideMove(e);
    },
    volSlideClick(e) {
      this.volSlideMove(e);
    },
    volMuted() {
      this.videoData.muted = !this.videoData.muted;
      this.volume.muted = this.videoData.muted;
    },
    setVol(val) {
      if (this.videoData) {
        this.volume.pos.current = val * this.volume.pos.width;
        this.volume.percent = val * 100;
        this.videoData.volume = val;
      }
    },
    fullScreen() {
      if (!this.state.fullScreen) {
        this.state.fullScreen = true;
        this.videoData.webkitRequestFullScreen();
      } else {
        this.state.fullScreen = false;
        document.webkitCancelFullScreen();
      }
      setTimeout(this.initVideo, 200);
    },
    mouseMoveAction(e) {
      if (this.volume.moving) {
        this.volSlideMove(e);
      }
      if (this.video.moving) {
        this.videoSlideMove(e);
      }
      this.contrlHider(e);
    },
    contrlHider(e) {
      const x = getMousePosition(e, "x");
      const y = getMousePosition(e, "y");
      if (!this.player.pos) return;
      if (
        x > this.player.pos.left &&
        x < this.player.pos.left + this.player.pos.width
      ) {
        if (
          y > this.player.pos.top + this.player.pos.height * 0.6 &&
          y < this.player.pos.top + this.player.pos.height
        ) {
          return this.mouseEnterVideo();
        }
      }
      return this.mouseLeaveVideo();
    },
    volSlideMove(e) {
      const x = getMousePosition(e) - this.volume.pos.start;
      if (x > 0 && x < this.volume.pos.width) {
        this.setVol(x / this.volume.pos.width);
      }
    },
    videoSlideMove(e) {
      const x = getMousePosition(e) - this.video.pos.start;
      if (x > 0 && x < this.video.pos.width) {
        this.video.pos.current = x;
        this.setVideoByTime(x / this.video.pos.width);
      }
    },
    mouseUpAction(e) {
      this.volume.moving = false;
      this.video.moving = false;
    },
    activePlaySpeedRate(rate) {
      return  this.state.playBackRate == rate ? 'bg-[#624b4bb8]' : ''
    },
    changePlaybackSpeed(rate) {
      this.videoData.playbackRate = parseFloat(rate)
      this.state.playBackRate =  this.videoData.playbackRate
      if(this.playbackSetting)
      {
        this.playbackSetting = false
        this.settingMenu = true
      }
    }
  },
};
</script>
<style>
.kidoo-video-container {
  position: relative;
  width: 100%;
  background-color: #000;
}

.kidoo-contrl-play-btn {
  position: relative;
  height: 100%;
  background: none;
  border: none;
  height: 2rem;
  width: 4rem;
  outline: none;
  vertical-align: top;
}
/* .kidoo-contrl-play-btn:hover {
  background-color: rgba(255, 255, 255, 0.27);
} */
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
  height: 100%;
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
::-webkit-media-controls {
  display: none !important;
}
video::-webkit-media-controls {
  display: none !important;
}
video::-webkit-media-controls-enclosure {
  display: none !important;
}
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
