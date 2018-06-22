<template>
  <div class="home">
    <form name="checkform" action="http://community.languagetool.org" method="post">

  <p id="checktextpara">
      <textarea id="checktext" name="text" style="width: 100%" rows="6">
      </textarea>
  </p>

  <div class="voiceText">
    <vue-speech v-if="active" lang="en" @onTranscriptionEnd="onEnd" />
  </div>

  <div style="opacity: 0;">
      <select name="lang" id="lang">
          <option value="en-US">English</option>
          <option value="de-DE">German</option>
          <option value="it">Italian</option>
      </select>
      <input type="submit" name="_action_checkText"
          value="Check Text" onClick="doit();return false;">
      <div id="feedbackErrorMessage" style="color: red;"></div>
  </div>


</form>

    <div class="guide" v-if="!active">Please open this page in Chrome(PC) and allow to use a microphone.<br>
    And then press "Start" button and start speaking in English.</div>
    <button class="startbtn" v-if="!active" v-on:click="toggle">Start</button>
    <button class="stopbtn" v-if="active" v-on:click="toggle">Stop</button>
  </div>
</template>

<script>
import Vue from 'vue'
import VueSpeech from 'vue-speech'

Vue.use(VueSpeech)




export default {
  name: 'home',
  data() {
    return {
      active: false,
      text: ''
    }
  },
  methods: {
    onEnd: function(transcription) {
      var iframeWindow = document.getElementById('checktext_ifr');
      var iframDom = iframeWindow.contentWindow.document.getElementById('tinymce');
      var innerTinymce = iframDom.children[0];
      var newArray = transcription.transcription;
      var newText = '';
      for (var i = 0; i < newArray.length; i++ ) {
        newText += newArray[i].charAt(0).toUpperCase() + newArray[i].slice(1) + '. ';
      }
      innerTinymce.innerHTML = newText;
      doit();
    },
    toggle: function() {
      this.active = !this.active
    }
  }
}
</script>

<style lang="css">

  .voiceText {
    display: none;
  }

  p {
    padding: 0px;
  }

  #checktext {
    width: 100%;
    height: 50vh;
    margin: 0 auto;
  }

  .startbtn {
    font-size: 24px;
    color: #fff;
    padding: 15px 60px;
    background: #69DD9D;
    border: none;
    border-radius: 10px;
    cursor: pointer;
  }

  .stopbtn {
    font-size: 24px;
    padding: 15px 60px;
    color: #fff;
    background: #ff6666;
    border: none;
    border-radius: 10px;
    cursor: pointer;
  }
</style>
