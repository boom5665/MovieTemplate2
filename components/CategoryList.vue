<template>
    <div class="" style="display: flex">
        <div style="margin-right: 60px">
            <div class="category-header">หมวดหมู่</div>
            <div class="category-list">
                <div class="category-single" style="font-size: 15px" v-for="(value, index) in categories" :key="'cate' + index">
                    <nuxt-link class="action-color" :to="'/' + _type + '/category/' + value.name_slug">{{ value.name_th }}</nuxt-link>
                </div>
            </div>
        </div>
        <div>
            <div class="category-header">ประเภท</div>
            <div class="category-list">
                <div class="category-single" v-for="(value, index) in types" :key="'type' + index">
                    <nuxt-link class="action-color" :to="'/' + _type + '/type/' + value.name_slug">{{ value.name_th }}</nuxt-link>
                </div>
            </div>
        </div>

        <!-- style="display: grid; grid-template-columns: 1fr 1fr 1fr 1fr" -->

        <!-- <div class="" @mouseover="onOver" @mouseleave="onLeave">
            <div class="bar1"></div>
            <div class="bar2"></div>
            <div class="bar3"></div>
            <b-dropdown id="dropdown-1" class="m-md-2 dropdown all">
                <b-dropdown-item :to="'/' + _type + '/category/' + value.name_slug" class="category-single drop-con" v-for="(value, index) in categories" :key="'cate' + index">
                    <span>{{ value.name_th }}</span>
                </b-dropdown-item>
            </b-dropdown>
        </div> -->
        <!-- <b-dropdown id="dropdown-1" text="หมวดหมู่" class="m-md-2 dropdown">
            <li class="category-single drop-con" v-for="(value, index) in categories" :key="'cate' + index">
                <nuxt-link :to="'/' + _type + '/category/' + value.name_slug" class="drop-a">{{ value.name_th }}</nuxt-link>
            </li>
        </b-dropdown> -->
    </div>
</template>

<script>
export default {
    props: {
        _type: {
            type: String,
            required: false,
            default: "movie",
        },
    },
    data() {
        return {
            categories: [],
            types: [],
        };
    },
    created() {
        this.getType();
        this.getCategory();
    },
    methods: {
        async getType() {
            const data = await this.$axios.$get(this._type + "/listtype");
            if (data.code == 200) this.types = data.result;
        },
        async getCategory() {
            const data = await this.$axios.$get(this._type + "/listcate");
            if (data.code == 200) this.categories = data.result;
        },
        onOver() {
            this.$refs.dropdown = true;
        },
        onLeave() {
            this.$refs.dropdown = false;
        },
    },
};
</script>

<style lang="scss">
.category-header {
    color: black;
    font-size: 20px;
}

.action-color {
    color: black;
    &:hover {
        color: #f5c518 !important;
    }
}
.category-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
}
</style>
