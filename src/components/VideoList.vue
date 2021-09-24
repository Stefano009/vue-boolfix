<template>
  <div>
      <ul>
          <li v-for="(video, index) in videosArray" :key="index">
              <span>il titolo è {{video.title}}</span>
              <span> il titolo originale è {{video.original_title}}</span>
              <span> la lingua è {{video.original_language}}</span>
              <span> il voto è {{video.vote_average}}</span>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
    name: 'VideoList',
    props: {
        queryResolver : String,
        videosArray : Array
    },
}
</script>

<style lang="scss" scoped>

</style>