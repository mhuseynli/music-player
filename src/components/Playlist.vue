<template>
  <section class="playlist">
    <h2>The Playlist</h2>
    <div class="song-list">
      <button
        v-for="song in songs"
        :key="song.src"
        @click="playSong(song)"
        :class="song.src == current.src ? 'song playing' : 'song'"
      >
        <div class="info">
          <h3>{{ song.title }}</h3>
          <p>By {{ song.artist }}</p>
        </div>
        <img src="@/assets/playing.gif" alt="playing" />
      </button>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    songs: {
      type: Array,
    },
    current: {
      type: Object,
    },
  },
  methods: {
    playSong(song) {
      this.$emit("playSong", song);
    },
  },
};
</script>

<style>

</style>
