<template>
  <div class="video">
    <div class="center">
      <div class="video-con">
        <!-- 左上角video -->
        <video id="video" @mouseover="showControls" @mouseout="hideControls" @click="changePosition" class="top-video" @canplay="canplay">
          <source src="https://h5player.bytedance.com/video/mp4/xgplayer-demo-360p.mp4">
        </video>
        <!-- bottom video -->
        <video id="bottomVideo" @mouseover="showControls" @mouseout="hideControls" @click="changePosition" class="bottom-video">
          <source src="https://videos.files.wordpress.com/0wGs9org/professional-video-production-southern-california_hd.mp4">
        </video>
        <!-- 播放按钮及滚动条 -->
        <div id="videoControls" @mouseover="showControls" @mouseout="hideControls">
          <div class="play-btn" v-if="isPlay" id="playBtn" @click="play"></div>
          <div class="play-btn stop-btn" v-if="!isPlay" id="playBtn" @click="play"></div>
          <div class="time">
            <span>{{currentTime | formatTime}}</span>
            <span>/</span>
            <span>{{totalTime | formatTime}}</span>
          </div>
          <!-- <button id="fullScreenBtn" title="FullScreen Toggle"> 全屏 </button> -->
          <div id="progressWrap" @mousedown="videoSeek">
            <div id="playProgress">
              <div id="circle"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "doubleVideo",
  data() {
    return {
      //显示播放/暂停
      isPlay: true,
      video: null,
      bottomVideo: null,
      progressFlag: null,
      progressWrap: null,
      playProgress: null,
      totalTime: 0,
      currentTime: 0,
    }
  },
  methods: {
    initVideo: function() {
      // 获取要操作的元素
      this.video = document.getElementById("video");
      this.bottomVideo = document.getElementById("bottomVideo");
      this.progressWrap = document.getElementById("progressWrap");
      this.playProgress = document.getElementById("playProgress");
    },
    // video互相切换
    changePosition: function() {
      let className = this.video.className;
      this.video.className = this.bottomVideo.className
      this.bottomVideo.className = className;
    },
    // 鼠标移入出现进度条
    showControls: function() {
      document.getElementById("videoControls").style.opacity = 1;
    },
    //鼠标移出隐藏滚动条
    hideControls() {
      document.getElementById("videoControls").style.opacity = 0;
    },
    //播放
    play() {
      this.isPlay = !this.isPlay;
      let bottomVideo = this.bottomVideo,
        video = this.video,
        _this = this;
      if (video.paused || video.ended) {
        if (video.ended) {
          video.currentTime = 0;
        }
        video.play();
        _this.progressFlag = setInterval(_this.getProgress, 60);
      } else {
        video.pause();
        clearInterval(_this.progressFlag);
      }
      if (bottomVideo.paused || bottomVideo.ended) {
        if (bottomVideo.ended) {
          bottomVideo.currentTime = 0;
        }
        bottomVideo.play();
        _this.progressFlag = setInterval(_this.getProgress, 60);
      } else {
        bottomVideo.pause();
        clearInterval(_this.progressFlag);
      }
    },
    //获取进度条信息
    getProgress() {
      this.currentTime = this.video.currentTime;
      let circle = document.getElementById("circle"),
        percent = this.video.currentTime / this.video.duration;
      this.playProgress.style.width = percent * (this.progressWrap.offsetWidth) - 2 + "px";
      circle.style.marginLeft = percent * (this.progressWrap.offsetWidth) - 2 + "px";
    },
    canplay() {
      this.totalTime = this.video.duration;
    },
    videoSeek(e) {
      if (this.video.paused || this.video.ended) {
        this.play();
        this.enhanceVideoSeek(e);
      } else {
        this.enhanceVideoSeek(e);
      }

    },
    //当视频播放完成或暂停时停止进度条查询及更新，清空定时器
    enhanceVideoSeek(e) {
      let _this = this;
      clearInterval(_this.progressFlag);
      let length = e.pageX - (_this.progressWrap.offsetLeft + 130 + 160 + 25);
      let percent = length / _this.progressWrap.offsetWidth;
      _this.playProgress.style.width = percent * (_this.progressWrap.offsetWidth) - 2 + "px";
      this.video.currentTime = percent * this.video.duration;
      this.bottomVideo.currentTime = percent * this.video.duration;
      _this.progressFlag = setInterval(_this.getProgress, 60);
    },

  },
  mounted() {
    this.initVideo();
  },
  filters: {
    'formatTime': function(value) {
      var second, minute, totalTime;
      second = value;
      minute = Math.floor(second / 60);
      second = Math.round(second % 60)
      minute += '';
      second += '';
      second = (second.length == 1) ? '0' + second : second;
      totalTime = minute + ':' + second;
      return totalTime;
    }
  }
}

</script>
<style>
.video {
  width: 100%;
  height: 100%;
  padding-top: 40px;
}

.left-tab {
  width: 130px;
  display: inline-block;
  position: relative;
}

.center {
  width: 680px;
  height: 595px;
  display: inline-block;
  text-align: center;
  vertical-align: top;
  margin-left: 160px;
}

.video-con {
  width: 100%;
  height: 450px;
  background-size: contain;
  position: relative;
  border:solid 1px #e4e4e4;
}

.top-video {
  position: absolute;
  top: 0;
  left: 12px;
  width: 150px;
  height: 150px;
  z-index: 999;
  cursor: pointer;
}

.bottom-video {
  position: absolute;
  top: 0;
  left: 12px;
  width: 610px;
  height: 410px;
  cursor: pointer;
}

.book-name {
  font-family: PingFangTC-Semibold;
  font-size: 20px;
  color: #FFFFFF;
  text-align: center;
  height: 60px;
  line-height: 50px;
}
/*进度条*/

.show {
  opacity: 1;
}

.hide {
  opacity: 0;
}

.time {
  width: 90px;
  color: #fff;
  display: inline-block;
}

#progressWrap {
  background-color: #DDDBD7;
  height: 3px;
  cursor: pointer;
  display: inline-block;
  width: 400px;
  vertical-align: top;
  margin-top: 17px;
}

#playProgress {
  background-color: #8A7F75;
  width: 400px;
  height: 3px;
  border-right: 2px solid blue;
  position: relative;
}

#circle {
  width: 8px;
  height: 8px;
  border-radius: 4px;
  position: absolute;
  top: -3px;
  background: #0F84F3;
}

#showProgress {
  background-color: ;
  font-weight: 600;
  font-size: 20px;
  line-height: 25px;
}

#videoControls {
  position: absolute;
  bottom: 11px;
  left: 12px;
  width: 610px;
  height: 40px;
  background: #333;
  text-align: left;
  opacity: 0.7;
}

.play-btn {
  display: inline-block;
  width: 16px;
  height: 16px;
  background: none;
  margin: 0 15px;
  margin-top: 10px;
  font-size: 16px;
  cursor: pointer;
  background: url('./assets/start.png');
}

.stop-btn {
  background: url('./assets/end.png');
}

</style>
