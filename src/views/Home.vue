<template>
  <div class="home">

<div class="content">
    <div class="guide" v-if="!active">
      <h1>思いつき英語発音文法アプリ</h1>
      <h2>発音と文法の練習が一気にできます。</h2>
      <p>＊このページをChrome(PC)で開いてマイクの使用を許可してください。</p>
      <p>「Start」ボタンを押して英語を話してみてください。</p>
      <p>文法が間違えていたら自動的にハイライトしてくれます。</p>
      <p>Please open this page in Chrome(PC) and allow to use a microphone.</p>
      <p>And then press "Start" button and start speaking in English.</p>
      <p>Your grammar mistakes will automatically be highlighted.</p>
    </div>

    <div class="encourage" v-if="active">
      <h2>難しいけど練習にはなるかと思います。</h2>
      <p>頑張って速めに正確に喋ってください笑</p>
      <p>発音と文法の練習です！！</p>
      <p>また、最初からしたい場合はページをリロードしてください。。。</p>
      <p>本当にごめんなさい。なおしますので。。ご勘弁を。。。(やり方よくわからないので誰かヘルプww)</p>
      Twitter : <a href="https://twitter.com/kentaro_koga">@kentaro_koga</a>
      <p>Please speak fast and clearly so this app will understand your English.</p>
      <p>Reload this page to restart.(I'm sorry. I'll work on this issue.lol)</p>
      <p>Enjoy!</p>
    </div>
    <button class="startbtn" v-if="!active" v-on:click="toggle">Start speaking English</button>
    <button class="stopbtn" v-if="active" v-on:click="reload">Reload this page</button>
    <h3 v-if="active">↓↓↓下に喋った英語が表示されます↓↓↓</h3>
    <!-- <button class="stopbtn" v-if="active" v-on:click="toggle">Stop</button> -->
  </div>

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
    },
    reload: function() {
      location.reload();
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
    height: 30vh;
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

  .guide {
    margin: 0 0 20px 0;
  }
</style>
