<template>
    <div class="w-96 mx-auto bg-white rounded shadow-lg flex min-h-180px">
        <img class="w-1/2" :src="song.thumbnail" alt="">
        <div class="w-1/2 flex flex-col justify-between">
            <a :href="song.url" target="_blank" class=" font-semibold mt-4 mx-4 text-left text-black no-underline">{{song.title}} </a>
            <!-- <p class="text-sm text-left">({{song.published}})</p> -->
            <!-- <a :href="song.url">View on Youtube</a> -->
            <ul class="list-reset flex justify-around">
                <li class="inline-block bg-grey-lighter rounded-full px-6 py-2 text-sm font-semibold text-grey-darker" v-for="tag in song.tags" :key="tag">{{tag}}</li>
            </ul>
            <button @click="addToDownloads" class="bg-orange rounded text-white m-4 h-10 hover:bg-orange-dark" :class="{'opacity-50 cursor-not-allowed hover:bg-orange': disableButton}">Add to Downloads</button>
        </div>
    </div>
</template>

<script>
export default {
  props: ["song", "disableButton"],
  methods: {
    addToDownloads() {
      console.log("clicked")
      this.$emit("add", this.song)
    }
  }
}
</script>

<style scoped>
</style>