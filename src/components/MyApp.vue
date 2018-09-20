<template>
  <div class="my-app">
    <h1>{{ appName }}</h1>
    <h2>Menu</h2>
    <ul>
      <li><router-link to='home'>Home</router-link></li>
      <li><router-link to='contact'>Contact</router-link></li>
    </ul>

    <input v-model="keyword" placeholder="keyword">
    <button id='book-search' v-on:click='bookSearch'>Search</button>
    <p>keyword is {{ keyword }}</p>

    <div class='search-result'>
      <div class='book' v-for='book in books' :key='book.isbn13'>
        <h1 class='book-title'>{{ book.title }}</h1>
        <h2 class='book-author'>{{ book.author }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
// import { setTimeout } from 'timers'
import * as axios from 'axios'
// import qs from 'qs'

/* let SAMPLE_BOOKS = [
  {
    id: 1,
    title: '数学ガール (数学ガールシリーズ 1)',
    author: '結城 浩',
    isbn13: '978-4797341379',
    icon_url: ''
  },
  {
    id: 2,
    title: '数学ガールの秘密ノート/式とグラフ',
    author: '結城 浩',
    isbn13: '978-4797374148',
    icon_url: ''
  }
] */

/* var bookSearchPromise = function () {
  return new Promise(
    function (resolve, reject) {
      setTimeout(function () {
        resolve(SAMPLE_BOOKS)
      }, 1000)
    }
  )
} */

/* var bookSearchAsync = async function (keyword) {
  function search (ret) {
    console.log(ret)
    console.log()
    return SAMPLE_BOOKS
  }

  return search(await axios.get('https://www.googleapis.com/books/v1/volumes?' + qs.stringify({q: keyword})))
} */

export default {
  name: 'MyApp',
  data () {
    return {
      appName: 'Welcome to Boo Koo',
      keyword: '',
      books: []
    }
  },
  computerd: {
  },
  methods: {
    bookSearch: function () {
      console.log('bookSearch called')
      var self = this
      axios.get('https://www.googleapis.com/books/v1/volumes', {
        params: {
          q: this.keyword
        }
      }).then(result => {
        console.log(result)
        // self.books.splice(0, self.books.length, ...books)
        if (result.status === 200) {
          var books = []
          result.data.items.forEach(element => {
            books.push({
              title: element.volumeInfo.title,
              author: element.volumeInfo.authors.join('/'),
              description: element.volumeInfo.description,
              isbn13: element.id
            })
          })

          console.log('books!')
          console.log(books)
          self.books.splice(0, self.books.length, ...books)
        }
      }).catch(error => {
        alert('error: bookSearch\n' + error)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

li .book {
  display: block;
}
</style>
