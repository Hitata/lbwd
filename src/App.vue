<template>
  <div id="app">
    <template v-if="messages">
      <div class="mainImage">
        <img src="../src/img/logo.png" alt="">
        <div class="mainMessage">
          <p>先日は素敵なバレンタインデーをありがとうございました！</p>
          <p>男性社員からそれぞれメッセージが届いていますので、</p>
          <p>ぜひ、気に入ったメッセージからギフトをお受取りください！</p>
        </div>
      </div>
      <div class="container">
        <div class="boxListWrapper">
          <template v-for="(row) in ['top', 'middle', 'bottom']">
            <ul class="boxList" :class="row" :key="row">
              <template v-for="(col) in ['first', 'second']">
                <li class="box" v-for="(message, messageIndex) in messages[row]" :class="{ opened: message.opened }" @click="open(message)">
                  <div class="name">{{ message.name }}</div>
                  <div class="boxImage">
                    <img class="boxImage-img" :src="`static/box${message.num}_closed.png`" v-if="!message.opened">
                    <img class="boxImage-img" :src="`static/box${message.num}_opened.png`" v-else>
                  </div>
                </li>
              </template>
            </ul>
          </template>
        </div>
      </div>
      <div class="modal" v-if="modal" @click="close">
        <div class="modal-content" @click="clickModalArea">
          <p class="modal-content-text">{{ modal.text }}</p>
          <p class="modal-content-name">by {{ modal.name }}</p>
          <a class="button" :href="`https://gift.starbucks.co.jp/c/${params.hash}`" target="_blank">ギフトを受け取る</a>
          <div class="close" @click="close">×</div>
        </div>
      </div>
      <footer>© LB Inc.</footer>
    </template>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      messages: null,
      modal: null,
      params: {}
    }
  },
  created () {
    fetch('static/messages.json').then(response => {
      return response.json();
    }).then(json => {
      this.messages = json;
      this.setParams();
      for (const name of ['top', 'middle', 'bottom']) {
        for (const v of this.messages[name]) {
          v.num = Math.floor(Math.random() * 3) + 1;
        }
      }
    });
  },
  methods: {
    setParams () {
      const urlParam = location.search.substring(1);
      if (urlParam) {
        urlParam.split('&').forEach(v => {
          const keyVal = v.split('=');
          this.params[keyVal[0]] = keyVal[1];
        });
      }
    },
    open (message) {
      message.opened = true;
      setTimeout(() => {
        this.modal = message;
      }, 100)
    },
    close (e) {
      this.modal = null;
    },
    clickModalArea (e) {
      e.stopPropagation();
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  max-width: 100%;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
body {
  font-size: 14px;
  background: #eceade;
  background: #ddd;
}
.container {
  max-width: 1000px;
  margin: auto;
  padding: 60px 0 80px;
  overflow: hidden;
}
.mainImage {
  background: url('img/bg.png');
  min-height: 360px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
  flex-direction: column;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
  justify-content: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
  align-items: center;
  border-top: 10px solid #112;
  border-bottom: 6px double #a1984a;
}
.mainImage img {
  width: 320px;
  margin: auto auto 20px;
}
.mainMessage {
  margin: 20px auto auto;
  padding: 0 20px;
  font-size: 14px;
  color: #818181;
  text-align: center;
  line-height: 1.9;
}
.boxListWrapper {
  position: relative;
  overflow: hidden;
}
.boxList {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  width: -webkit-max-content;
  width: -moz-max-content;
  width: max-content;
  max-width: none;
}
.boxList.top,
.boxList.bottom {
  animation: scroll1 30s linear 0s infinite normal;
}
.boxList.middle {
  animation: scroll2 30s linear 0s infinite normal;
}
.box {
  position: relative;
  background: rgba(255, 255, 255, 0.4);
  width: 200px;
  height: 200px;
  margin: 10px;
  cursor: pointer;
  border-radius: 100px;
  overflow: visible;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
  justify-content: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
  align-items: center;
}
.box .name {
  position: absolute;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  color: #fff;
  font-size: 13px;
  padding: 2px 10px;
  border-radius: 100px;
}
.boxImage {
  width: 60%;
}
.boxImage-img {
  width: 100%;
}
.modal {
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
  justify-content: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
  align-items: center;
  z-index: 1000;
  -webkit-animation: fadeIn .2s ease 0s;
  animation: fadeIn .2s ease 0s;
}
.modal-content {
  position: relative;
  background: #fff;
  color: #a1984a;
  padding: 50px 60px;
  margin: auto;
  width: 90%;
  max-width: 400px;
  line-height: 1.9;
  font-size: 16px;
}
.modal-content-name {
  text-align: center;
  margin-top: 10px;
  font-size: 14px;
}
.button {
  display: block;
  color: #fff;
  text-decoration: none;
  text-align: center;
  padding: 5px 10px;
  margin: 20px 0 0;
  font-size: 16px;
  border: 2px solid #a1984a;
  color: #a1984a;
  cursor: pointer;
}
.close {
  position: absolute;
  top: 10px;
  right: 10px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
  justify-content: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
  align-items: center;
  width: 24px;
  height: 24px;
  border: 2px solid #a1984a;
  border-radius: 100px;
  font-size: 13px;
  font-weight: bold;
  line-height: 1;
  cursor: pointer;
}
footer {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
  justify-content: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
  align-items: center;
  background: #112;
  min-height: 120px;
  font-size: 12px;
  color: #818181;
  text-align: center;
}
@-webkit-keyframes scroll1 {
  0% {
    -webkit-transform: translateX(0);
            transform: translateX(0);
  }
  100% {
    -webkit-transform: translateX(-50%);
            transform: translateX(-50%);
  }
}
@keyframes scroll1 {
  0% {
    -webkit-transform: translateX(0);
    transform: translateX(0);
  }
  100% {
    -webkit-transform: translateX(-50%);
            transform: translateX(-50%);
  }
}
@-webkit-keyframes scroll2 {
  0% {
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
  }
  100% {
    -webkit-transform: translateX(0);
            transform: translateX(0);
  }
}
@keyframes scroll2 {
  0% {
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
  }
  100% {
    -webkit-transform: translateX(0);
    transform: translateX(0);
  }
}
@-webkit-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@media (max-width: 600px) {
  .mainImage img {
    width: 240px;
  }
  .box {
    width: 120px;
    height: 120px;
  }
}
</style>

