<template>
 <q-layout> 
   <!-- Header -->
  <div slot="header" class="toolbar">
    <!-- opens left-side drawer using its ref -->
    <button class="hide-on-drawer-visible" @click="$refs.leftDrawer.open()">
      <i>menu</i>
    </button>
    <q-toolbar-title :padding="1">
      Books
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

    <div class="layout-view">
	    <div class="panel panel-default">

	      <div class="panel-heading">
	        <h5 class="panel-title">Add New Books</h5>
	      </div>

	      <div class="panel-body">
	         <form id="form" class="form-inline" v-on:submit.prevent="addBook">
	          <div class="form-group">
	            <label for="bookTitle">Title:</label>
	            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title" required>
	          </div>
	          <div class="form-group">
	            <label for="bookAuthor">Author:</label>
	            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author" required>
	          </div>
	          <div class="form-group">
	            <label for="bookUrl">Url:</label>
	            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url" required>
	          </div>
	          <div class="form-group">
	          	<button type="submit" class="primary">Add Book</button>
	          </div>
	        </form>
	      </div>
	    </div>

	    <div class="panel panel-default">
		      <div class="panel-heading">
		        <h5 class="panel-title">Book List</h5>
		      </div>

		      <div class="panel-body">
		        <table class="q-table">
		          <thead>
		            <tr>
		              <th>Title</th>
		              <th>Author</th>
		              <th>Action</th>
		            </tr>
		          </thead>
		          <tbody>
		            <tr v-for="book in books">
		              <td><a v-bind:href="book.url">{{book.title}}</a></td>
		              <td>{{book.author}}</td>
		              <td><button class="primary circular small" @click="removeBook(book)"> <i>remove</i></button></td>
		            </tr>
		          </tbody>
		        </table>
		      </div>
	    </div>
     </div>
  </q-layout> 
</template>

<script>

import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: 'AIzaSyD4HkuaF1RkSTkI9qSGNvCO-qMpPXHbtmA',
  authDomain: 'books-7b1a6.firebaseapp.com',
  databaseURL: 'https://books-7b1a6.firebaseio.com',
  projectId: 'books-7b1a6',
  storageBucket: '',
  messagingSenderId: '345782842014'
}

let app = Firebase.initializeApp(config)
let db = app.database()
console.log(db)
let booksRef = db.ref('books')

export default {
  firebase: {
    books: booksRef
  },

  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'http://'
      }
    }
  },

  methods: {
    addBook: function () {
      booksRef.push(this.newBook)
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = 'http://'
    },

    removeBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Book remove successfully')
    }
  }
}
</script>

<style>
</style>
