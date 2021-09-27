<template>
    <li @change="console">
            <span>il titolo è {{video.title}}</span>
            <img :src="poster(video.poster_path)" :alt="video.title">
            <span> il titolo originale è {{video.original_title}}</span>
            <country-flag :country='getFlag(video.original_language)' size='small'/>
            <!-- capire perchè si duplicano le chiavi -->
            <i v-for="(n, index) in stars(video.vote_average)" :key="index" class="fas fa-star"></i>
            <div v-if="(stars(video.vote_average) < 5)" class="emptyStar">
                <i v-for="(n, index) in (5-(stars(video.vote_average)))" :key="index" class="far fa-star"></i>
            </div>
    </li>
</template>

<script>
export default {
    name:'Card',
    props: ["card"],
}
</script>

<style lang="scss" scoped>

</style>