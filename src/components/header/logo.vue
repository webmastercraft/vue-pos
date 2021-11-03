<template>
  <div class="profile-nav onhover-dropdown p-0 mr-0">
    <div class="media profile-media">
      <div class="media-body notifi">
        <p class="mb-0 font-roboto">
          Notifications <i class="middle fa fa-angle-down"></i>
        </p>
      </div>
    </div>
    <ul class="profile-dropdown onhover-show-div notifi-menu">
      <li>
        <a href="/shops/creat"><feather type="bell"></feather><span>Admin </span></a>
      </li>
      <li>
        <a href="/users/profile"><feather type="mail"></feather><span>Messages </span></a>
      </li>
      <li>
        <a href="/shops/profile"><feather type="align-center"></feather><span>Comments</span></a>
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
