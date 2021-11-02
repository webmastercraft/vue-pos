<template>
  <div class="profile-nav onhover-dropdown p-0 mr-0">
    <div class="media profile-media">
      <div class="media-body">
        <p class="mb-0 font-roboto">
          Notifications <i class="middle fa fa-angle-down"></i>
        </p>
      </div>
    </div>
    <ul class="profile-dropdown onhover-show-div">
      <li>
        <a href="/shops/creat"><span>Admmin Notifications </span></a>
      </li>
      <li>
        <a href="/users/profile"><span>Messages </span></a>
      </li>
      <li>
        <a href="/shops/profile"><span>Comments</span></a>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: 'Logo',
    methods: {
      toggle_sidebar() {
        this.$store.dispatch('menu/opensidebar');
      },
    },
  };
</script>
