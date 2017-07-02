<template>
  <div id="app" class="app">
    <textarea :value="input" @input="update"></textarea>
    <div v-html="compiledMarkdown"></div>
  </div>
</template>

<script>
  import MarkdownIt from 'markdown-it'
  import debounce from 'lodash.debounce'

  const md = new MarkdownIt()

  export default {
    name: 'app',
    data() {
      return {
        input: '# hello'
      }
    },
    computed: {
      compiledMarkdown: function () {
        return md.render(this.input, { sanitize: true })
      }
    },
    methods: {
      update: debounce(function (e) {
        this.input = e.target.value
      }, 300)
    }
  }
</script>

<style>
  html, body, #app {
    margin: 0;
    height: 100%;
    font-family: 'Helvetica Neue', Arial, sans-serif;
    color: #333;
  }

  textarea, #app div {
    display: inline-block;
    width: 49%;
    height: 100%;
    vertical-align: top;
    box-sizing: border-box;
    padding: 0 20px;
  }

  textarea {
    border: none;
    border-right: 1px solid #ccc;
    resize: none;
    outline: none;
    background-color: #f6f6f6;
    font-size: 14px;
    font-family: 'Monaco', courier, monospace;
    padding: 20px;
  }

  code {
    color: #f66;
  }
</style>
