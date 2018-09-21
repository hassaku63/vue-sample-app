<template>
  <div class="my-app">
    <h1>{{ appName }}</h1>
    <h2>Menu</h2>
    <ul>
      <li><router-link to='home'>Home</router-link></li>
      <li><router-link to='contact'>Contact</router-link></li>
    </ul>

    <input v-model="keyword" placeholder="keyword">
    <button id='book-search' @click='bookSearch'>Search</button>
    <p>keyword is {{ keyword }}</p>

    <div class='search-result container'>
      <div class='book row' v-for='book in books' :key='book.isbn13'>
        <div class='book-info four columns'>
          <h3 class='book-title'>{{ book.title }}</h3>
          <h4 class='book-author'>{{ book.author }}</h4>
        </div>
        <div class='book-detail seven columns'>
          <p class='book-description'>{{ book.shortDescription }}</p>
        </div>
        <div class='book-action one columns'>
          <button class='book-rental' name='isbn13' :value='book.isbn13' @click='onRentalClicked'>Rental</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as axios from 'axios'
import * as _ from 'lodash'

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
      /*
        TODO:
        - [ ] 空文字で検索した場合、不正なクエリになりレスポンスが400になる
      */
      console.log('bookSearch called')
      var self = this
      axios.get('https://www.googleapis.com/books/v1/volumes', {
        params: {
          q: this.keyword
        }
      }).then(result => {
        /*
          TODO:
          - [ ] 検索の一致が0件だった場合のハンドリングが必要。今はforeachで例外が出る
        */
        console.log(result)
        // self.books.splice(0, self.books.length, ...books)
        if (result.status === 200) {
          var books = []
          result.data.items.forEach(item => {
            books.push({
              title: _.get(item, 'volumeInfo.title', 'No title'),
              author: _.get(item, 'volumeInfo.authors', ['No author']).join('/'),
              categories: _.get(item, 'volumeInfo.categories', []),
              shortDescription: _.get(item, 'searchInfo.textSnippet', 'No description'),
              description: _.get(item, 'volumeInfo.description', 'No descroption'),
              imageLink: _.get(item, 'volumeInfo.imageLinks.smallThumbnail', undefined),
              isbn13: _.get(item, 'id') // volumeInfo.industryIdentifiers
            })
          })

          console.log('books!')
          console.log(books)
          self.books.splice(0, self.books.length, ...books)
        }
      }).catch(error => {
        alert('error: bookSearch\n' + error)
      })
    },
    onRentalClicked: function (event) {
      console.log('onRentalClicked')
      console.log(event.srcElement.value)
      console.log(event.value)
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

.book-info {
  text-align: left;
}

.book-detail {
  text-align: left;
}

h3.book-title {

}

h4.book-author {

}

p.book-description {

}

button.book-rental {

}
</style>
