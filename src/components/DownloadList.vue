<template>
  <div @click="expandList" class="bg-white border-t-2 border-grey fixed pin-b pin-x bottom-bar overflow-auto" :class="{ 'slide-in': showSongs }">
    <div class="max-w-md mx-auto flex justify-between items-center m-4">
      <p class="text-2xl">{{songsInList}} {{songsInList === 1 ? 'song' : 'songs'}} ready for download</p>
      <div>
        <button @click.stop="downloadAll" type="button" class="rounded bg-orange font-bold leading-normal text-white py-2 px-5 hover:bg-orange-dark" :class="{'opacity-50 cursor-not-allowed hover:bg-orange': disableButton}" :disabled="disableButton">Download All</button>
      </div>
    </div>
    <ul v-show="showSongs" class="max-w-md mx-auto list-reset text-left">
      <li v-for="song in toDownload" :key="song.url" class="flex justify-between my-2">
        <p class="text-lg w-5/6">{{song.title}} <span class="text-xs">({{song.duration}})</span></p> <span @click.stop="removeSong(song)" class="text-sm text-grey-dark w-1/6 underline text-center">Remove</span>
      </li>

    </ul>
  </div>
</template>

<script>
import axios from "axios"
export default {
  props: ["toDownload", "isDownloading"],
  data() {
    return {
      showSongs: false
    }
  },
  computed: {
    songsInList() {
      return this.toDownload.length
    },
    disableButton() {
      return this.isDownloading || this.songsInList === 0
    }
  },
  methods: {
    expandList() {
      let div = this.$el
      console.log(div)
      this.showSongs = !this.showSongs
    },
    downloadAll() {
      this.$emit("start-download")
      let songs = this.toDownload
      const SERVER_URL = process.env.VUE_APP_SERVER_URL
      console.log("songs", songs)
      axios({
        method: "post",
        url: SERVER_URL + "/download",
        data: {
          songs: songs
        }
      })
        .then(res => {
          console.log("response", res)
          this.$emit("got-urls", res.data)
        })
        .catch(err => console.log("error", err))
    },
    removeSong(song) {
      console.log("song in dl list", song.title)
      this.$emit("remove", song)
    }
  }
}
</script>

<style scoped>
.bottom-bar {
  height: 80px;
  transition: height 0.75s;
}

.slide-in {
  height: 300px;
}
</style>