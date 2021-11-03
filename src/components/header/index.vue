<template>
    <div class="header-wrapper row m-0">
        <SearchBar />
        <Logo />
        <div class="left-header col horizontal-wrapper pl-0">
        </div>
        <div class="header_app_name"><h3>Dailli App</h3></div>
        <div class="nav-right col-8 pull-right right-.page-wrapper .page-header p-0">
            <ul class="nav-menus">
                <li class="language-nav">
                    <Language />
                </li>
                <Profile />
            </ul>
        </div>
    </div>
</template>

<script>
import { mapState } from 'vuex';
import Profile from './profile';
import Logo from './logo';
import SearchBar from './search';
import Language from './language';

export default {
    name: 'Search',
    components: {
        Language,
        Profile,
        Logo,
        SearchBar,
    },
    data() {
        return {
            bookmark: false,
        };
    },
    computed: {
        ...mapState({
            menuItems: (state) => state.menu.searchData,
            megamenuItems: (state) => state.menu.megamenu,
            searchOpen: (state) => state.menu.searchOpen,
        }),
    },
    methods: {
        search_open() {
            this.$store.state.menu.searchOpen = true;
        },
        bookmark_open() {
            this.bookmark = !this.bookmark;
        },
    },
};
</script>
