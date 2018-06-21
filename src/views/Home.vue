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

  <div>
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
    <button v-if="!active" v-on:click="toggle">Start Speaking</button>
    <button v-if="active" v-on:click="toggle">Stop Speaking</button>
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
        newText += newArray[i] + '. ';
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
    margin: 0 auto;
  }
</style>
