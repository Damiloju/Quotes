<template>
  <q-layout
    ref="layout"
    view="lHh Lpr fff"
    :left-class="{'bg-grey-11': true}"
  >
    <q-transition
            appear
            enter="bounceIn"
            leave="flipOutX"
    >
    <div class="welcome center" v-if=" step === 1 ">
      <blockquote class="emphasize shadow-up-4 welcome">
        <p>Hey mate thanks for downloading this application</p>
        <p> - Hope you enjoy it</p>

        <q-btn color="primary" rounded class="glossy" @click="step = 2">
          Inspire Me
        </q-btn>
      </blockquote>
    </div>
    </q-transition>
    <template class="main-app" v-if="step == 2">
      <q-toolbar slot="header" class="glossy text-center" color="tertiary">
        <q-btn
          flat
          @click="$refs.layout.toggleLeft()"
        >
          <q-icon name="home" />
        </q-btn>

        <q-toolbar-title>
          Quotes
        </q-toolbar-title>
      </q-toolbar>

      <div slot="left">
        <!--
          Use <q-side-link> component
          instead of <q-item> for
          internal vue-router navigation
        -->

        <q-list no-border link inset-delimiter>

        </q-list>
      </div>


      <q-transition
            appear
            enter="flipInX"
            leave="flipOutX"
      >
        <div v-if="show">
          <blockquote class="emphasize shadow-up-4">
            <p>{{ quotes.quote }}</p>
            <p> - {{ quotes.quoteAuthor }}</p>

            <q-btn color="primary" rounded @click="generateQuote" class="right glossy">
              Another Quote
            </q-btn>

          </blockquote>
       </div>

      </q-transition>
    </template>

  </q-layout>
</template>

<script>
  /* eslint-disable no-undef */
  import { quotes } from '../quotes.js'
  import {
    QLayout,
    QTransition,
    QToolbar,
    QToolbarTitle,
    QBtn,
    QIcon,
    QList,
    QListHeader,
    QItem,
    QItemSide,
    QItemMain
  } from 'quasar'

  export default {
    name: 'index',
    components: {
      QLayout,
      QToolbar,
      QToolbarTitle,
      QBtn,
      QTransition,
      QIcon,
      QList,
      QListHeader,
      QItem,
      QItemSide,
      QItemMain
    },
    data () {
      return {
        params: {
          method: 'getQuote',
          format: 'json',
          lang: 'en'
        },
        quotes: {
          quote: 'Be good, be fun, be loving, be caring, but most importantly be yourself',
          quoteAuthor: 'Yusuf Oluwadamiloju'
        },
        show: true,
        quotesArray: quotes,
        userName: '',
        step: 1,
        dere: false
      }
    },
    created () {
      if (!(localStorage.getItem['userName'] === null)) {
        this.userName = JSON.parse(localStorage.getItem('userName'))
        console.log('its here mate')
        this.dere = true
      }
    },
    methods: {
      saveDataToLocalStorage (data) {
        // Parse the serialized data back into an array of objects
        this.userName = JSON.parse(localStorage.getItem('userName'))
        // Push the new data (whether it be an object or anything else) onto the array
        this.userName = data
        // Alert the array value
        // alert(this.todo) // Should be something like [Object array]
        // Re-serialize the array back into a string and store it in localStorage
        localStorage.setItem('userName', JSON.stringify(this.userName))
      },
      generateQuote () {
        this.show = false
        let randNumber = Math.floor(Math.random() * this.quotesArray.length)
        let quote = this.quotesArray[randNumber]
        console.log(quote)
        this.quotes.quote = quote.quote
        this.quotes.quoteAuthor = quote.quoteAuthor
        setTimeout(() => {
          this.show = true
        }, 1000)
      },
      setUser () {
        this.step = 2
        this.saveDataToLocalStorage(this.userName)
      }
    }
}
</script>

<style>
  body{
    background-image: url("../assets/black_paper.png");
    font-family: "fira", cursive;
  }
  blockquote {
    background: #efebe7;
    border-left: 10px solid #ccc;
    position: absolute;
    border-radius: 10px;
    margin: 1.5em 10px;
    padding: 0.5em 10px;
    quotes: "\201C""\201D""\2018""\2019";
    font-family: 'mine', cursive;
    font-size: 20px;
  }
  blockquote:before {
    color: #ccc;
    content: open-quote;
    font-size: 4em;
    line-height: 0.1em;
    margin-right: 0.25em;
    vertical-align: -0.4em;
  }

  blockquote p {
    display: inline;
  }

  .right {
    float: right;
    margin-left: 10em;
  }

  /*.welcome {*/
    /*margin-top: 10em;*/
    /*margin-left: 20em;*/
  /*}*/
</style>
