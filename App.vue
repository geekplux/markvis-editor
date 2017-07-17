<template>
  <div id="app" class="app">
    <div class="sidebar">
      <h1>Markvis</h1>
      <h2>Make visualization in markdown.</h2>
      <ul>
        <li><a href="https://markvis.js.org">Home</a></li>
        <li><a href="https://markvis-editor.js.org">Editor</a></li>
        <li><a href="https://github.com/geekplux/markvis">Source</a></li>
      </ul>
      <div class="footer">
        <h2>MIT © <a href="https://github.com/geekplux">GeekPlux</a></h2>
      </div>
    </div>
    <textarea :value="input" @input="update"></textarea>
    <div class="preview" ref="preview" v-html="compiledMarkdown"></div>
  </div>
</template>

<script>
  import MarkdownIt from 'markdown-it'
  import vis from 'markvis'
  import debounce from 'lodash.debounce'
  import * as d3 from 'd3'

  const md = new MarkdownIt().use(vis, { d3 })
  const defaultContent = `
    # Hello World

    I'm **testing**!

    ## Test


        console.log('inline code');


    \`\`\`js
    console.log('language javascript')
    \`\`\`

    \`\`\`vis
    layout: bar
    data: [
      { key: 0, value: 5 },
      { key: 1, value: 4 },
      { key: 2, value: 7 },
      { key: 3, value: 2 },
      { key: 4, value: 4 },
      { key: 5, value: 8 },
      { key: 6, value: 3 },
      { key: 7, value: 6 }
    ]
    \`\`\`

    \`\`\`vis
    layout: line
    data: [
      { key: 0, value: 45 },
      { key: 1, value: 100 },
      { key: 2, value: 70 },
      { key: 3, value: 20 },
      { key: 4, value: 30 },
      { key: 5, value: 80 },
      { key: 6, value: 10 },
      { key: 7, value: 60 }
    ]
    \`\`\`


    \`\`\`vis
    layout: pie
    radius: 150
    data: [
      { key: 0, value: 5 },
      { key: 1, value: 4 },
      { key: 2, value: 7 },
      { key: 3, value: 2 },
      { key: 4, value: 4 },
      { key: 5, value: 8 },
      { key: 6, value: 3 },
      { key: 7, value: 6 }
    ]
    \`\`\`

    \`\`\`vis
    layout: no_layout
    data: [
      { key: 0, value: 5 },
      { key: 1, value: 4 },
      { key: 2, value: 7 },
      { key: 3, value: 2 },
      { key: 4, value: 4 },
      { key: 5, value: 8 },
      { key: 6, value: 3 },
      { key: 7, value: 6 }
    ]
    \`\`\`
  `

  export default {
    name: 'app',
    data() {
      return {
        input: defaultContent
      }
    },
    computed: {
      compiledMarkdown () {
        return md.render(this.input, {
          width: window.innerWidth / 2,
          height: window.innerWidth / 4
        })
      }
    },
    methods: {
      update: debounce(function (e) {
        this.input = e.target.value
      }, 300)
    },
  }
</script>

<style>
  html, body, .app {
    margin: 0;
    height: 100%;
    font-family: 'Helvetica Neue', Arial, sans-serif;
    color: #333;
  }

  .app {
    display: flex;
  }

  textarea, .preview {
    height: 100%;
    vertical-align: top;
    box-sizing: border-box;
    padding: 0 20px;
    overflow-y: scroll;
  }

  textarea {
    flex: 4;
    border: none;
    border-right: 1px solid #ccc;
    resize: none;
    outline: none;
    background-color: #f6f6f6;
    font-size: 14px;
    font-family: 'Monaco', courier, monospace;
    padding: 20px;
    box-shadow: 0 1px 5px #aaa inset;
  }

  .sidebar {
    flex: 2;
    padding: 20px;
    box-shadow: 10px 10px 10px #000;
  }

  .sidebar h2 {
    color: #bbb;
    font-style: italic;
    font-size: 16px;
  }

  .sidebar ul {
    margin-top: 30px;
    padding: 0;
    font-size: 14px;
  }

  .sidebar li {
    list-style: none;
  }

  .sidebar li a {
    color: #aaa;
  }

  .sidebar .footer {
    position: absolute;
    bottom: 100px;
  }

  .preview {
    flex: 7;
  }

  code {
    color: #f66;
  }
</style>
