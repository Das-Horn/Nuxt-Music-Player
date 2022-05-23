<script>
export default {
  methods : {
    // Calling the function openFile() in the parent component.
    callFile(event) {
      // console.log(event);
      this.$parent.openFile();
    }
  }
}
</script>

<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand>
            <BIconMusicPlayer></BIconMusicPlayer>
      </b-navbar-brand>
      <b-navbar-brand href="#">Music Player</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-navbar-nav class="ml-auto">
            <b-button @click="callFile" size="m" class="my-2 my-sm-0" >Upload</b-button>
          <b-nav-item-dropdown text="Theme" right>
            <b-dropdown-item >light</b-dropdown-item>
            <b-dropdown-item >dark</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
    </b-navbar>
  </div>
</template>

