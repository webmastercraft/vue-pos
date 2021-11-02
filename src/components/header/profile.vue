<template>
  <li class="profile-nav onhover-dropdown p-0 mr-0">
    <div class="media profile-media">
      <div class="media-body">
        <p class="mb-0 font-roboto">
          Welcome Ali <i class="middle fa fa-angle-down"></i>
        </p>
      </div>
    </div>
    <ul class="profile-dropdown onhover-show-div">
      <li>
        <a href="/shops/creat"><feather type="archive"></feather><span>Shop Profile </span></a>
      </li>
      <li>
        <a href="/users/profile"><feather type="user"></feather><span>User Profile </span></a>
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
