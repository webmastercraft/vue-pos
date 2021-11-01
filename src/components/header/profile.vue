<template>
  <li class="profile-nav onhover-dropdown p-0 mr-0">
    <div class="media profile-media">
      <img
        class="b-r-10"
        src="../../assets/images/dashboard/profile.jpg"
        alt=""
      />
      <div class="media-body">
        <span>Emay Walter</span>
        <p class="mb-0 font-roboto">
          Admin <i class="middle fa fa-angle-down"></i>
        </p>
      </div>
    </div>
    <ul class="profile-dropdown onhover-show-div">
      <li>
        <a href="/shops/creat"><feather type="archive"></feather><span>New EShop </span></a>
      </li>
      <li>
        <a href="/users/profile"><feather type="user"></feather><span>Account </span></a>
      </li>
      <li>
        <a href="/shops/profile"><feather type="shopping-cart"></feather><span>My Shop</span></a>
      </li>
      <li>
        <a href="#"
          ><feather type="file-text"></feather><span>Cashier</span></a
        >
      </li>
      <li>
        <a href="#">
          <div class="dropdown shop_list">
            <feather type="list"></feather><span>Shops list</span>
            <div class="dropdown-content">
              <a href="/shops/profile">Link 1</a>
              <a href="/shops/profile">Link 2</a>
              <a href="/shops/profile">Link 3</a>
            </div>
          </div>
        </a>
      </li>
      <li @click="openCustomizerSetting('settings')">
        <a href="#"><feather type="settings"></feather><span>Settings</span></a>
      </li>
      <li>
        <a @click="logout"
          ><feather type="log-in"></feather><span>Log out</span></a
        >
      </li>
    </ul>
  </li>
</template>

<script>
  import firebase from 'firebase';
  import UserAuth from '../../auth/js/index';

  export default {
    name: 'Profile',
    methods: {
      logout: function() {
        firebase
          .auth()
          .signOut()
          .then(() => {
            UserAuth.Logout();
            this.$router.replace('/auth/login');
          });
      },

      openCustomizerSetting(val) {
        this.$store.state.menu.customizer = val;
      },
    },
  };
</script>
