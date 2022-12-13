<template>
    <div id="footer">
        <b-container class="h-100" style="max-width: 100%">
            <div class="d-flex justify-content-center h-100" style="padding: 30px; align-items: flex-start;">
                <b-col cols="12" lg="3" xl="3" style="align-items: center;">
                    <nuxt-link to="/" style="text-align: center">
                        <nuxt-img format="webp" :src="logo" width="500" height="100" :alt="SEOTitle" class="footer-logo-m" />
                    </nuxt-link>
                </b-col>
                <b-col cols="12" lg="3" xl="3" class="footer-m">
                    <div class="menu-btn"><CategoryList :_isAV="true" /></div>
                </b-col>
                <b-col cols="12" lg="2" xl="2" style="color: black;" class="footer-m">
                    <div class="">เรื่องล่าสุด</div>
                    <div class="">Samaritan (2022) ซามาริทัน</div>
                    <div class="">Samaritan (2022) ซามาริทัน</div>
                    <div class="">Samaritan (2022) ซามาริทัน</div>
                    <div class="">Samaritan (2022) ซามาริทัน</div>
                </b-col>
                <b-col cols="12" lg="2" xl="2" style="color: black" class="footer-m">
                    <div class="">ติดต่อ</div>
                    <div class="">Line : xxxxxxxxx</div>
                    <div class="">Facebook : xxxxxxxx</div>
                </b-col>
                <!-- <div class="back-to-top" @click="scrollToTop()"><b-icon-arrow-up class="icon" /></div> -->
            </div>
        </b-container>
    </div>
</template>

<script>
import { mapState } from "vuex";
export default {
    computed: {
        ...mapState({
            SEOTitle: (state) => state.SEOTitle,
            logo: (state) => state.logo,
        }),
    },
    methods: {
        scrollToTop() {
            window.scrollTo({ top: 0, behavior: "smooth" });
        },
    },
};
</script>
