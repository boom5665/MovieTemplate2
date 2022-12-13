<template>
    <div class="swiper-bullet-cate">
        <vue-glide  :breakpoints="{ 800: { perView: 2 }, 1200: { perView: 5 } }" v-model="active" v-bind="carouselOptions" v-if="movieList.length > 0">
            <!-- <template slot="control" >
                <button class="arrow-left" data-glide-dir="<"><nuxt-img src="/icon-left.png" /></button>
                <button class="arrow-right" data-glide-dir=">"><nuxt-img src="/icon-right.png" /></button>
            </template> -->

            <vue-glide-slide v-for="(value, index) in movieList" :key="index"> <SingleMovieBlock :_obj="value" /></vue-glide-slide>
        </vue-glide>
    </div>
</template>

<script>
export default {
    data() {
        return {
            carouselOptions: {
                autoplay: 3500,
                hoverpause: true,
                perView: 7,
                focusAt: "center",
                type: "carousel",
                perTouch: 5,
            },

            active: 0,
            movieList: [],
        };
    },

    mounted() {
        this.getMovies();
        // console.log(this._slug);
    },
    computed: {
        _slug() {
            return this.$route.params.slug;
        },
    },
    methods: {
        // async asyncData({ params, $axios }) {
        //     const res = await $axios.$get("manga/listcate?slug=" + params.slug);
        //     const typeObj = res.result[0];
        //     return { typeObj };
        // },
        getMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmoviesimilar", {
                    category: [43],
                    perpage: 10,
                    page: 1,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        // console.log(response.result);
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.arrow-right {
    background: transparent;
    border: none;
    font-size: 50px;
    color: white;
    position: absolute;
    right: -12%;
    top: 65%;
}
.arrow-left {
    background: transparent;
    border: none;
    font-size: 50px;
    color: white;
    position: absolute;
    right: -7%;
    top: 65%;
}
</style>

