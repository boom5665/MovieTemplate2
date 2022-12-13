<template>
    <div>
        <swiperhome class="display-home-page-m" />
        <swiperpage class="display-home-page-pc" />
        <div class="list-container">
            <div class="" v-if="movieList.length">
                <div class="loader-container" v-show="loadingList">
                    <nuxt-img format="webp" src="/loader.png" alt="loader" />
                </div>
            </div>
            <div class="home-con border-bgwhite" v-if="movieList" >
                <b-col cols="6" lg="3" xl="3" class="home-dis-top">
                    <div class="dis-between" style="text-align: left">
                        <h3 class="list-title-black">หนัง ACTION</h3>
                        <h6 class="list-title-black">NEW MOVIE ACTION</h6>
                    </div>
                    <div class="dis-between">
                        <nuxt-link to="/movie/category/action">
                            <h5 class="list-title-black"><nuxt-img format="webp" src="/add-movie.png" alt="loader" /> ดูเพิ่มเติม</h5>
                        </nuxt-link>
                    </div>
                </b-col>
                <b-col cols="12" lg="12" xl="8">
                    <!-- <div style="display: flex">
                        <h6 class="list-title">ยอดนิยม</h6>
                        <h6 class="list-title">อัพเดทล่าสุด</h6>
                        <h6 class="list-title">หนังชนโรง</h6>
                        <h6 class="list-title">เร็วๆ นี้</h6>
                    </div> -->
                    <swipercate-recom />
                </b-col>
            </div>
            <div class="home-con border-bgblue" v-if="movieList">
                <b-col cols="12" lg="12" xl="8">
                    <swipercate-update />
                </b-col>
                <b-col cols="6" lg="3" xl="3" class="home-dis-top">
                    <div class="dis-between" style="text-align: right">
                        <h2 class="list-title-white">หนัง ADVENTURE</h2>
                        <h6 class="list-title-white">NEW MOVIE ADVENTURE</h6>
                    </div>
                    <div class="dis-between">
                        <nuxt-link to="/movie/category/adventure">
                            <h5 class="list-title-white"><nuxt-img format="webp" src="/add-movie-white.png" alt="loader" /> ดูเพิ่มเติม</h5>
                        </nuxt-link>
                    </div>
                </b-col>
            </div>

            <div class="home-con border-bgbluelight" v-if="movieList">
                <b-col cols="6" lg="3" xl="3" class="home-dis-top">
                    <div class="dis-between" style="text-align: left">
                        <h2 class="list-title-white">หนังใหม่ FANTASY</h2>
                        <h6 class="list-title-white">NEW MOVIE FANTASY</h6>
                    </div>
                    <div class="dis-between">
                        <nuxt-link to="/movie/category/fantasy">
                            <h5 class="list-title-white"><nuxt-img format="webp" src="/add-movie-white.png" alt="loader" /> ดูเพิ่มเติม</h5>
                        </nuxt-link>
                    </div>
                </b-col>
                <b-col cols="12" lg="12" xl="8">
                    <swipercate-new />
                </b-col>
            </div>

            <div class="home-con border-bgblue"  v-if="movieList">
                <b-col cols="12" lg="12" xl="8">
                    <swipercate-av />
                </b-col>
                <b-col cols="6" lg="3" xl="3" class="home-dis-top">
                    <div class="dis-between" style="text-align: right">
                        <h3 class="list-title-white">หนังAV</h3>
                        <h6 class="list-title-white">AV PORNSTAR ONLINE</h6>
                    </div>
                    <div class="dis-between">
                        <nuxt-link to="/av/category/big-tits">
                            <h5 class="list-title-white"><nuxt-img format="webp" src="/add-movie-white.png" alt="loader" /> ดูเพิ่มเติม</h5>
                        </nuxt-link>
                    </div>
                </b-col>
            </div>

            <div class="movie-not-found" v-else>
                <span v-show="!loadingList">ไม่พบหนัง</span>
                <div class="loader-container" v-show="loadingList">
                    <nuxt-img format="webp" src="/loader.png" alt="loader" />
                </div>
            </div>
            <div class="loader-container" v-show="loadingList">
                <nuxt-img format="webp" src="/loader.png" alt="loader" />
            </div>
        </div>
    </div>
</template>

<script>
import swipercateUpdate from "./swipercate-update.vue";
export default {
    components: { swipercateUpdate },
    props: {
        _title: {
            type: String,
            required: true,
        },
        _type: {
            type: String,
            required: false,
            default: "",
        },
        _category: {
            type: String,
            required: false,
            default: "",
        },
        _isHot: {
            type: Boolean,
            required: false,
            default: false,
        },
        _isSearch: {
            type: Boolean,
            required: false,
            default: false,
        },
        _search: {
            type: String,
            required: false,
            default: "",
        },
        _isAV: {
            type: Boolean,
            required: false,
            default: false,
        },
    },
    data() {
        return {
            loadingList: false,
            movieList: [],
            currentPage: 1,
            maxPage: 1,
        };
    },
    created() {
        if (this._isHot) {
            this.getHotMovies();
        } else if (this._isSearch) {
            this.getSearchMovie();
        } else {
            this.getMovies();
        }
    },
    mounted() {},
    watch: {
        currentPage() {
            if (this._isHot) {
                this.getMovies();
            } else if (this._isSearch) {
                this.getSearchMovie();
            } else {
                this.getHotMovies();
            }
        },
        _search() {
            this.getSearchMovie();
        },
    },
    methods: {
        getHotMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 6,
                    page: this.currentPage,
                    search: this._search,
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
        getMovies() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 18,
                    page: this.currentPage,
                    search: this._search,
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
        getSearchMovie() {
            const self = this;
            this.loadingList = true;
            this.$axios
                .$post("movie/listmovie", {
                    order: "ID",
                    orderby: "DESC",
                    types: "",
                    category: "",
                    year: 0,
                    perpage: 30,
                    page: this.currentPage,
                    search: this._search,
                })
                .then(function (response) {
                    if (response.code == 200) {
                        self.movieList = response.result;
                        self.maxPage = response.page_total;
                    }
                    self.loadingList = false;
                });
        },
        checkIndex(index) {
            if ((index >= this.currentPage && index - 3 < this.currentPage) || (index <= this.currentPage && index + 3 > this.currentPage)) return true;
            return false;
        },
    },
};
</script>


