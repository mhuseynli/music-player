<template>
  <div>
    <section class="player">
      <h2 class="song-name">{{ current.title }}</h2>
      <p class="artist-name">{{ current.artist }}</p>
      <div @click="jumpTime" ref="progress" class="progress">
        <div
          :style="{ flexBasis: percent + '%' }"
          class="progress_filled"
        ></div>
      </div>
      <div class="controls">
        <button @click="prev">
          <img class="control" src="@/assets/prev.png" alt="prev" />
        </button>
        <button v-if="isPlaying" @click="pause">
          <img class="control" src="@/assets/pause.png" alt="pause" />
        </button>
        <button v-else @click="play">
          <img class="control" src="@/assets/play.png" alt="play" />
        </button>
        <button @click="next">
          <img class="control" src="@/assets/next.png" alt="next" />
        </button>
        <div class="volume">
          <p>Volume</p>
          <input
            type="range"
            class="istyle"
            min="0"
            max="100"
            v-model.number="volume"
          />
        </div>
      </div>
    </section>
    <Playlist :songs="songs" :current="current" @playSong="play($event)" />
  </div>
</template>

<script>
import Playlist from "./Playlist.vue";

export default {
  name: "Player",
  components: {
    Playlist,
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      volume: 100,
      percent: 0,
      songs: [
        {
          title: "Rajaz",
          artist: "Camel",
          src: require("@/assets/Camel - Rajaz.mp3"),
        },
        {
          title: "Mystic Queen",
          artist: "Camel",
          src: require("@/assets/Camel - Mystic Queen.mp3"),
        },
        {
          title: "Stationary Traveller",
          artist: "Camel",
          src: require("@/assets/Camel - Stationary Traveller.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
  watch: {
    volume() {
      this.player.volume = this.volume / 100;
    },
  },
  methods: {
    handleProgress() {
      this.percent = (this.player.currentTime / this.player.duration) * 100;
    },
    jumpTime(e) {
      this.player.currentTime =
        (e.offsetX / this.$refs.progress.offsetWidth) * this.player.duration;
    },
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
        this.index = this.songs.indexOf(song);
      }
      this.player.play();
      this.player.addEventListener("ended", this.next);
      this.player.addEventListener("timeupdate", this.handleProgress);
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
};
</script>

<style>
.player {
  text-align: center;
  color: white;
}
.player .song-name {
  font-size: 32px;
  font-weight: 400;
}
.player .artist-name {
  font-weight: 300;
  font-size: 20px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
.progress {
  flex: 10;
  position: relative;
  display: flex;
  flex-basis: 100%;
  height: 3px;
  background: rgba(0, 0, 0, 0.5);
  cursor: pointer;
  margin-top: 10px;
}

.progress_filled {
  width: 50%;
  background: #626262;
  flex: 0;
  flex-basis: 0.1%;
}
.controls {
  margin-top: 20px;
}
.controls button {
  padding: 0 10px;
}
.controls button img {
  width: 65px;
  border-radius: 50%;
  padding: 5px;
}
.controls button:active .control {
  background-color: #626262;
}

.volume p {
  font-weight: 100;
  letter-spacing: 1px;
}

input[type="range"] {
  -webkit-appearance: none;
  margin: 10px 0;
  padding: 13px;
  border-radius: 20px;
}
input[type="range"]:focus {
  outline: none;
}
input[type="range"]::-webkit-slider-runnable-track {
  width: 100%;
  height: 4px;
  cursor: pointer;
  animate: 0.2s;
  box-shadow: 0px 0px 0px #000000;
  background: #000000;
  border-radius: 50px;
  border: 0px solid #ffffff;
}
input[type="range"]::-webkit-slider-thumb {
  border: 1px solid #000000;
  height: 15px;
  width: 15px;
  border-radius: 50px;
  background: #000000;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -6px;
}
input[type="range"]:focus::-webkit-slider-runnable-track {
  background: #000000;
}
input[type="range"]::-moz-range-track {
  width: 100%;
  height: 4px;
  cursor: pointer;
  animate: 0.2s;
  box-shadow: 0px 0px 0px #000000;
  background: #000000;
  border-radius: 50px;
  border: 0px solid #ffffff;
}
input[type="range"]::-moz-range-thumb {
  border: 1px solid #000000;
  height: 15px;
  width: 15px;
  border-radius: 50px;
  background: #000000;
  cursor: pointer;
}
input[type="range"]::-ms-track {
  width: 100%;
  height: 4px;
  cursor: pointer;
  animate: 0.2s;
  background: transparent;
  border-color: transparent;
  color: transparent;
}
input[type="range"]::-ms-fill-lower {
  background: #000000;
  border: 0px solid #ffffff;
  border-radius: 100px;
  box-shadow: 0px 0px 0px #000000;
}
input[type="range"]::-ms-fill-upper {
  background: #000000;
  border: 0px solid #ffffff;
  border-radius: 100px;
  box-shadow: 0px 0px 0px #000000;
}
input[type="range"]::-ms-thumb {
  border: 1px solid #000000;
  height: 15px;
  width: 15px;
  border-radius: 50px;
  background: #000000;
  cursor: pointer;
}
input[type="range"]:focus::-ms-fill-lower {
  background: #000000;
}
input[type="range"]:focus::-ms-fill-upper {
  background: #000000;
}

.playlist {
  margin-top: 20px;
}
.playlist h2 {
  color: #fff;
  font-weight: 200;
}
.song-list {
  margin-top: 10px;
}
.song-list button:first-child {
  border-top: 1px solid #626262;
}
.song-list button {
  width: 100%;
  text-align: left;
  color: white;
  padding: 10px;
  border-bottom: 1px solid #626262;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.song-list button h3 {
  font-weight: 500;
  font-size: 18px;
}
.song-list button p {
  font-style: italic;
}
.song img {
  display: none;
}
.playing img {
  display: block;
  width: 20px;
}
</style>
