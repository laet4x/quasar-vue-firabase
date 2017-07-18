<template>
  <q-layout>
    <div slot="header" class="toolbar">
      <!-- opens drawer below
      <button class="hide-on-drawer-visible" @click="$refs.drawer.open()">
        <i>menu</i>
      </button>
      -->
      <q-toolbar-title :padding="1">
        Title
      </q-toolbar-title>
    </div>


    <!-- Navigation Tabs -->
  <q-tabs slot="navigation">
    <q-tab icon="home" route="/" exact replace>Home</q-tab>
    <q-tab icon="info" route="/about" exact replace>Info</q-tab>
    <q-tab icon="add" route="/books" exact replace>Add Books</q-tab>
  </q-tabs>
   

    <!-- Drawer
    <q-drawer ref="drawer">
      <div class="toolbar">
        <q-toolbar-title>
          Drawer Title
        </q-toolbar-title>
      </div>

      <div class="list no-border platform-delimiter">
        <q-drawer-link icon="mail" :to="{path: '/', exact: true}">
          Link
        </q-drawer-link>
      </div>
    </q-drawer>
    -->

    <router-view class="layout-view"></router-view>

    <!-- Footer
    <div slot="footer" class="toolbar"></div>
    -->
  </q-layout>
</template>

<script>
export default {
  data () {
    return {}
  }
}
</script>

<style>
</style>
