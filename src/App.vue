<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase Applictaion</h1>
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3>Add Book</h3>
          </div>
          <div class="panel-body">
            <form id="form" class="form-inline" v-on:submit.prevent="addBook">
              <div class="form-group">
                <label for ="bookTitle">Title</label>
                <input type="text" id="bookTitle" class="form-control" v-model="newBook.title"/>
              </div>
              <div class="form-group">
                <label for ="bookAuthor">Author</label>
                <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author"/>
              </div>
              <div class="form-group">
                <label for ="bookUrl">Url</label>
                <input type="text" id="bookUrl" class="form-control" v-model="newBook.url"/>
              </div>
              <input type="submit" class="btn btn-primary" value="Add Book" />
            </form>
          </div>
        </div>
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3>Books List</h3>
          </div>
          <div class="panel-body">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>
                    Title
                  </th>
                  <th>
                    Author
                  </th>
                  <th>
                    Delete
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="book in books">
                  <td>
                    <a v-bind:href="book.url">{{book.title}}</a>
                  </td>
                  <td>
                    {{book.author}}
                  </td>
                  <td>
                    <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'
import toastr from 'toastr'
import Firebase from 'firebase'

let config = {
   apiKey: "AIzaSyCyJUr6kAMRNCqWEQo3Qd9AykJQ9PqhPDY",
   authDomain: "vuejs-firebase01-58598.firebaseapp.com",
   databaseURL: "https://vuejs-firebase01-58598.firebaseio.com",
   projectId: "vuejs-firebase01-58598",
   storageBucket: "vuejs-firebase01-58598.appspot.com",
   messagingSenderId: "815469748518"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function(){
      booksRef.push(this.newBook)
      this.newBook.title = "";
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.sucsess("Book removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
