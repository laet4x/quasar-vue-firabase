<template>
   <q-layout>
  <!-- Header -->
  <div slot="header" class="toolbar">
    <!-- opens left-side drawer using its ref -->
    <button class="hide-on-drawer-visible" @click="$refs.leftDrawer.open()">
      <i>menu</i>
    </button>
    <q-toolbar-title :padding="1">
      About Me
    </q-toolbar-title>
    <!-- opens right-side drawer using its ref -->
    <!-- <button class="hide-on-drawer-visible" @click="$refs.rightDrawer.open()">
        <i>menu</i>
     </button> -->
  </div>
  <!-- Navigation Tabs -->
  <q-tabs slot="navigation">
    <q-tab icon="home" route="/" exact replace>Home</q-tab>
    <q-tab icon="info" route="/about" exact replace>Info</q-tab>
    <q-tab icon="add" route="/books" exact replace>Add Books</q-tab>
  </q-tabs>
  <!-- Left-side Drawer -->
  <q-drawer ref="leftDrawer">
    <div class="toolbar">
      <q-toolbar-title>
        Menu
      </q-toolbar-title>
    </div>
    <div class="list no-border platform-delimiter">
      <q-drawer-link icon="mail" :to="{path: '/', exact: true}">
        Link
      </q-drawer-link>
    </div>
  </q-drawer>
  <!-- IF USING subRoutes only: -->
  <router-view class="layout-view"></router-view>
  <!-- OR ELSE, IF NOT USING subRoutes: -->
  <div class="layout-view">
  	
	 <div class="card">
	  <div class="card-content">
	      <div v-if="users">
	      	  <img :src="users.avatar_url" width="100" height="100">
		      <p><strong>Name:</strong>{{users.name}}</p>
		      <p><strong>Url:</strong><a :href="users.html_url"> {{users.html_url}} </a></p>
		      <p><strong>Public Repo:</strong>{{users.public_repos}}</p>
		      <p><strong>Followers:</strong>{{users.followers}}</p>
		      <p><strong>Following:</strong>{{users.following}}</p>
		      <p><strong>Bio:</strong>{{users.bio}}</p>
		  </div>
		  <ul v-if="errors && errors.length">
		    <li v-for="error of errors">
		      {{error.message}}
		    </li>
		  </ul>
	  </div>
	</div>

  </div>
  <!-- Right-side Drawer -->
 <!--  <q-drawer ref="rightDrawer" right-side>
    ...
  </q-drawer> -->
  <!-- Footer -->
  <div slot="footer" class="toolbar">
    @company name
  </div>
 </q-layout> 
</template>

<script>

import Vue from 'vue'
import layout from '../../templates/layout.vue'

Vue.component('my-layout', layout)

import axios from 'axios'
export default {
  data: () => ({
    users: {},
    followers: [],
    errors: []
  }),

  // Fetches posts when the component is created.
  created () {
    axios.get(`https://api.github.com/users/laet4x`)
    .then(response => {
      // JSON responses are automatically parsed.
      this.users = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
    // async / await version (created() becomes async created())
    //
    // try {
    //   const response = await axios.get(`http://jsonplaceholder.typicode.com/posts`)
    //   this.posts = response.data
    // } catch (e) {
    //   this.errors.push(e)
    // }
  }
}
</script>

<style>
</style>
