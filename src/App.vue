<template lang="html">
    <div class="app">
      <Corner bg-color="#3399ff" color="#f7f7f7"></Corner>
      <div class="img-container">
        <img height="150" width="150" src="https://user-images.githubusercontent.com/6414178/44472944-dd525880-a661-11e8-9c56-3e73395109c3.png" />
      </div>
      <div class="header">
        zero-width-lib
      </div>
      <div class="title install">
        0. Install
      </div>
      <div class="img-container">
        <img width="40%" src="https://user-images.githubusercontent.com/6414178/44473787-b39a3100-a663-11e8-9df6-f4faed559e76.png" />
      </div>
      <div class="title">
        1. Encode
      </div>
      <div class="flex-container">
        <div class="flex-item-lg">
          <div class="textarea-container">
            <textarea v-model="visIn"
                      placeholder="Type visible text"></textarea>
          </div>
          <div class="textarea-container">
            <textarea v-model="hidIn"
                      placeholder="Type invisible text (support😆)"></textarea>
          </div>
        </div>
        <div class="flex-item-sm">
          <div class="blue-bar">
          </div>
          <div class="blue-bar">
          </div>
        </div>
        <div class="flex-item-lg">
          <div class="textarea-container">
            <textarea class="readonly"
                      v-model="encoded"
                      readonly
                      placeholder="Copy the encoded text here and paste to the decode section below"></textarea>
          </div>
        </div>
      </div>
      <div class="title">
        2. Decode
      </div>
      <div class="flex-container">
        <div class="flex-item-lg">
          <div class="textarea-container">
            <textarea v-model="encodedIn"
                      placeholder="Paste the encoded text"></textarea>
          </div>
        </div>
        <div class="flex-item-sm">
          <div class="blue-bar">
          </div>
          <div class="blue-bar">
          </div>
        </div>
        <div class="flex-item-lg">
          <div class="textarea-container">
            <textarea class="readonly"
                      v-model="decoded.vis"
                      readonly
                      placeholder="Decoded visible text"></textarea>
          </div>
          <div class="textarea-container">
            <textarea class="readonly"
                      v-model="decoded.hid"
                      readonly
                      placeholder="Decoded invisible text"></textarea>
          </div>
        </div>
      </div>
      <div class="title">
        3. Escape Regex
      </div>
      <div class="flex-container">
        <div class="flex-item-lg">
          <div class="textarea-container">
            <textarea v-model="escapeIn"
                      placeholder='Type "sexual" and "violent"'></textarea>
          </div>
        </div>
        <div class="flex-item-sm">
          <div class="blue-bar">
          </div>
          <div class="blue-bar">
          </div>
        </div>
        <div class="flex-item-lg">
          <div class="textarea-container">
            <div class="textarea" v-html="filtered">
            </div>
          </div>
          <div class="textarea-container">
            <div class="textarea" v-html="filteredEscaped">
            </div>
          </div>
        </div>
      </div>
      <div class="more">
        <div class="dot-container" @click="toGithub">
          <div class="dot">
          </div>
          <div class="dot">
          </div>
          <div class="dot">
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import * as zeroWidthLib from 'zero-width-lib';
import Corner from './Corner';

export default {
  name: 'app',
  components: {
    Corner
  },
  data: () => {
    return {
      visIn: '',
      hidIn: '',
      encodedIn: '',
      escapeIn: ''
    }
  },
  computed: {
    encoded: function() {
      return zeroWidthLib.encode(this.visIn, this.hidIn);
    },
    decoded: function() {
      const extracted = zeroWidthLib.extract(this.encodedIn);
      return {
        vis: extracted.vis,
        hid: zeroWidthLib.decode(extracted.hid)
      }
    },
    filtered: function() {
      if(this.escapeIn.length === 0) {
        return '<span class="placeholder">Without zero width characters, "sexual" and "violent" will be crossed out</span>';
      }
      return this.filter(this.escapeIn);
    },
    filteredEscaped: function() {
      if(this.escapeIn.length === 0) {
        return '<span class="placeholder">With zero width characters, "sexual" and "violent" will be fine</span>';
      }
      return this.filter(zeroWidthLib.split(this.escapeIn));
    }
  },
  methods: {
    escapeHtml(unsafe) {
      const tagToReplace = {
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        '"': '&quot;',
        '\'' : '&#039;'
      };
      return unsafe.replace(/[&<>"']/g, matched => {
        return tagToReplace[matched] || matched;
      });
    },
    filter(text) {
      return this.escapeHtml(text).replace(/sexual|violent/g, matched => {
        return `<span class="crossed">${matched}</span>`;
      });
    },
    toGithub() {
      location.href = 'https://github.com/yuanfux/zero-width-lib';
    }
  }
}
</script>

<style lang="scss">
  html {
    font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI" ,Roboto, "Helvetica Neue", Arial, sans-serif;
  }

  html,
  body,
  .app {
    height: auto;
  }

  .app {
    margin: 20px 100px 50px 100px;
  }

  body {
    background-color: #f7f7f7;
  }

  .header {
    text-align: center;
    margin-bottom: 15px;
    font-size: 30px;
    font-weight: bold;
    color: #287acc;
  }

  .textarea {
    width: 100%;
    font-size: 16px;
    height: 150px;
    border-radius: 3px;
    padding: 10px;
    box-sizing: border-box;
    border: none;
    background-color: #4da6ff;
    color: #fff;
  }

  textarea {
    width: 100%;
    font-size: 16px;
    height: 150px;
    border-radius: 3px;
    padding: 10px;
    box-sizing: border-box;
    border: none;
    resize: none;
    font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI" ,Roboto, "Helvetica Neue", Arial, sans-serif;
    &:not(.readonly):focus {
      outline: none;
      box-shadow: 0 0 0 2px #3399ff;
    }
    &.readonly {
      background-color: #4da6ff;
      color: #fff;
      &:focus {
        outline: none;
      }
    }
  }

  .title {
    text-align: center;
    color: #3399ff;
    &.install {
      margin-top: 20px;
    }
    &:not(.install) {
      margin: 20px 0;
    }
  }

  .img-container {
    text-align: center;
  }

  .flex-container {
    display: flex;
    align-items: center;
  }

  .flex-item-sm {
    flex-grow: 1;
    flex-basis: 0;
  }

  .flex-item-lg {
    flex-grow: 10;
    flex-basis: 0;
  }

  .textarea-container:not(:last-child) {
    .textarea {
      margin-bottom: 3px;
    }
  }

  .blue-bar {
    &:not(:last-child) {
      margin-bottom: 30px;
    }
    height: 10px;
    background-color: #3399ff;
  }

  .crossed {
    text-decoration: line-through;
  }

  .placeholder {
    color: #777777;
  }

  .more {
    margin-top: 20px;
    text-align: center;
    .dot-container {
      cursor: pointer;
      display: inline-block;
      width: 100px;
      .dot {
        display: inline-block;
        background-color: #4da6ff;
        width: 10px;
        height: 10px;
        border-radius: 50%;
      }
      &:hover {
        .dot {
          animation: strong-color 560ms ease-in-out forwards;
        }
      }
    }
  }

  @keyframes strong-color {
    0% {
      background-color: #4da6ff;
    }
    50% {
      background-color: #3399ff;
    }
    100% {
      background-color: #1a8cff;
    }
  }

  ::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
    color: #777777;
    opacity: 1; /* Firefox */
  }

  :-ms-input-placeholder { /* Internet Explorer 10-11 */
      color: #777777;
  }

  ::-ms-input-placeholder { /* Microsoft Edge */
      color: #777777;
  }
</style>