<template>
  <div class="main" style="position: relative; margin: 0 0; width: 100%;">
    <img src="../assets/background_test.jpg" style="width: 100%; height: 100%">
    <div class="middleContent" style="position: absolute; top:20%; left: 0; width:100%; display: flex;">
<!--      <div v-show="friendFlag === 1" class="friend-list" style="width: 210px; margin-left: 0; margin-right: -210px">-->
<!--        <div style="width: 100%;"><img src="../assets/friendList/top.png" alt="top_info"></div>-->
<!--        <div style="width: 100%; height: 300px; background-color: pink; margin: 0 0;">-->
<!--          <p>好友(/78)</p>-->
<!--        </div>-->
<!--        <div style="width: 100%;"><img src="../assets/friendList/bottom.png" alt="bottom_info"></div>-->
<!--      </div>-->
      <div class="wishesModle" style="width: 500px; height: 100px;overflow: scroll; margin-left: 0;background-color: white">
        <div v-for="(item,id) in info.slice(0,wishCount)" :key="id">
          <div v-if="item">{{item.wish}}</div>
        </div>
      </div>
      <div v-show="banghuiFlag === 1" class="banghuiModle">
        <div class="btn" style="margin-left: 920px; margin-top: 40px;" @click="close()">
          <img src="../assets/close.png">
        </div>
        <div id="banghuiBlock">
          <div v-for="(photo,id) in imgInfo" :key="id" >
            <div v-if="photo" class="card" style="max-width:200px; margin-right: 20px; margin-bottom: 20px">
              <img v-if="photo" :src="getImgSrc(photo)" :preview="0" :preview-text="photo.username" alt="Card image cap" >
              <div class="card-text">{{photo.username}}</div>
            </div>
          </div>
        </div>
      </div>
      <div v-show="messageFlag === 1" class="messageModle" style="margin: 0 auto; display: flex">
        <div class="left-message">
          <div style="height: 250px; overflow: scroll">
              <audio id="voice" :src="getVoiceSrc()" hidden></audio>
              <div v-for="(item,id) in info" :key="id">
                <div v-if="item" class="messageList" style="display: flex;" @click="messageIndex=item.id;handlePlay('voice','musicAudio')">
                    <div><img src="../assets/message/tubiao.png" style="width: 40px" alt="message_icon"></div>
                    <div style="margin-left: 10px; padding-top: 10px; font-size: large; height: 53px" >
                      {{item.username}}
                    </div>
                </div>
              </div>
          </div>
        </div>
        <div class="right-message">
          <div style="position: relative; width: 385px; height: 397px;">
            <img src="../assets/message/right.png" alt="right message background">
            <div class="btn" style="position: absolute; top: -10px; right: 20px;" @click="close()">
              <img src="../assets/close.png">
            </div>
            <div style="position: absolute; top: 48px; left: 100px; text-align: left" v-if="info[messageIndex]">
              <p style="margin: 0 0">{{info[messageIndex].username}}</p>
              <p style="margin: 0 0">生日快乐</p>
            </div>
            <div style="max-width:300px; max-height: 180px; overflow: scroll; position: absolute; top: 120px; left: 30px; text-align: left" v-if="info[messageIndex]">
              <p style="margin: 0 0">{{info[messageIndex].sentence}}</p>
            </div>>
            <div class="btn" v-if="info[messageIndex]" style="position: absolute; top: 277px; left: 280px" @click="handlePlay('audio')"><img src="../assets/message/pl.png"> </div>
          </div>
        </div>
      </div>
      <div v-show="zhanjiFlag === 1" class="zhanjiModle" style="margin: 0 auto; position: relative">
        <img :src="getZhanjiSrc()">
        <div class="btn" style="position: absolute; top:5px; right: -10px" @click="close()">
          <img src="../assets/close.png">
        </div>
        <div class="btn" v-on:click="zhanjiCount = (zhanjiCount+2)%3" style="position: absolute;top:270px; left: 35px; width: 30px; height: 30px;"></div>
        <div class="btn" v-on:click="zhanjiCount = (zhanjiCount+1)%3" style="position: absolute;top:270px; right: 35px; width: 30px; height: 30px;"></div>
      </div>
      <div v-show="musicFlag === 1" class="musicModle">
        <div class="btn" style="margin-left: 755px; margin-top: 5px" @click="close()">
          <img src="../assets/close.png">
        </div>
        <div style="display: flex; flex-wrap: wrap; justify-content: left; overflow: scroll; width: 570px; max-height: 260px; margin-top: 80px; margin-left: 20px">
          <audio id="musicAudio" :src="getMusicSrc()" hidden></audio>
          <div v-for="(item,id) in musicInfo" :key="id" style="width:183px; height: 80px; margin-right: 5px; margin-bottom: 5px;">
            <div class="card" style="width:183px; height: 80px; padding: 0 0; background-color: #cac0b4" @click="musicIndex=item.id;handlePlay('musicAudio','voice')">
              <div class="card-title">{{item.music_name}}</div>
              <div class="card-body" style="padding-top: 0;">
                <p style="padding: 0 0; margin: 0 0">演唱者：{{item.username}}</p>
                <p style="padding: 0 0; margin: 0 0">原唱：{{item.ori_singer}}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="corner" style="position: absolute; bottom: 0">
      <div class="left-corner">
<!--        <img id="friend" src="../assets/corner-icon/friend.png" alt="friend"  v-on:click="friendFlag = (friendFlag+1)%2">-->
        <img id="messages" src="../assets/corner-icon/messages.png" alt="messages" v-on:click="change('messageFlag',messageFlag)">
        <img id="banghui" src="../assets/corner-icon/bangHui.png" alt="banghui" v-on:click="change('banghuiFlag',banghuiFlag);">
      </div>
      <div class="right-corner">

        <img id="zhanji" src="../assets/corner-icon/zhanji.png" alt="zhanji" v-on:click="change('zhanjiFlag',zhanjiFlag)">
        <img id="photos" src="../assets/corner-icon/photos.png" alt="photos">
        <img id="music" src="../assets/corner-icon/music.png" alt="music" v-on:click="change('musicFlag',musicFlag)">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  components: {},
  data(){
    return{
      timer:null,
      info:{
        "id": 0,
        "username": "岚棠",
        "friendType": "student",
        "job": "qixiu",
        "wish": "wish test",
        "sentence": "sentecne test",
      },
      musicInfo:{
        "id": 0,
        "username": "岚棠",
        "ori_singer": "吴青峰",
        "music_name": "星光"
      },
      imgInfo:{
        "id": 1,
        "username": "不会奶的盆栽"
      },
      zhanjiCount: 0,
      wishCount: 0,
      // friendFlag: 0,
      banghuiFlag: 0,
      messageFlag: 0,
      zhanjiFlag: 0,
      musicFlag: 0,
      messageIndex: -1,
      musicIndex: -1
    }
  },
  methods:{
    handlePlay(id, tempid) {
      var audio = document.getElementById(id);

      var temp = document.getElementById(tempid);
      if(temp!==null && !temp.paused){
        temp.pause();
      }

      if(audio!==null){
        if(audio.paused){
          audio.play();
        }else{
          audio.pause();
        }
      }
    },
    getData(){
      this.$http.get("../info.json").then(res=>{
        this.info = res.data.data;
      })
    },
    getMusicData(){
      this.$http.get("../music.json").then(res=>{
        this.musicInfo = res.data.data;
      })
    },
    getImgData(){
      this.$http.get("../imgInfo.json").then(res=>{
        this.imgInfo = res.data.data;
        this.$previewRefresh();
      })
    },
    getZhanjiSrc(){
      return require("../assets/zhanji/"+this.zhanjiCount+".png");
    },
    getVoiceSrc(){
      if (this.messageIndex === -1)
        return null;
      else
        return require("../assets/sentenceVoice/"+this.info[this.messageIndex].username+".mp3");
    },
    getMusicSrc(){
      if (this.musicIndex === -1)
        return null;
      else
        return require("../assets/sentenceVoice/"+this.info[this.musicIndex].username+".mp3");
    },
    getImgSrc(item){
      return item.username && require("../assets/banghui/"+item.username+".jpg");
    },
    change(name,flag){
      this.close();
      switch (name){
        case "banghuiFlag":
          this.banghuiFlag = (flag + 1) %2;
          break;
        case "messageFlag":
          this.messageFlag = (flag + 1) %2;
          break;
        case "zhanjiFlag":
          this.zhanjiFlag = (flag + 1) %2;
          break;
        case "musicFlag":
          this.musicFlag = (flag + 1) %2;
          break;
      }
    },

    close(){
      this.zhanjiCount = 0;
      this.banghuiFlag = 0;
      this.messageFlag = 0;
      this.zhanjiFlag = 0;
      this.musicFlag = 0;
      this.messageIndex = -1;
      this.musicIndex = -1;
    }

  },
  created() {
    clearInterval(this.timer);
    this.timer = null;
    this.getData();
    this.getMusicData();
    this.getImgData();
    this.timer = setInterval(()=>{
      this.wishCount++;
      if (this.wishCount >=8)
        this.wishCount = 8;
    },1000);
  },
  beforeDestroy() {
    clearInterval(this.timer);
    this.timer = null;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main{
  /*background-image: url("../assets/friendList/testBackgroundImg.jpg");*/
}
.corner{
  display: flex;
  width: 100%;
}

.left-corner{
  width: 5%;
  float: left;
  margin-right: auto;
}
.right-corner{
  width: 15%;
  float: right;
}
#friend:hover{
  content: url("../assets/corner-icon/friend_highlight.png");
}
#banghui:hover{
  content: url("../assets/corner-icon/banghui_highlight.png");
}
#messages:hover{
  content: url("../assets/corner-icon/messages_highlight.png");
}
#zhanji:hover{
  content: url("../assets/corner-icon/zhanji_highlight.png");
}
#photos:hover{
  content: url("../assets/corner-icon/photos_highlight.png");
}
#music:hover{
  content: url("../assets/corner-icon/music_highlight.png");
}

.card .card-img{
  margin: 0 0;
  padding: 0 0;
}

.left-message{
  background-image: url("../assets/message/left.png");
  width: 252px;
  height: 397px;
  padding-top: 80px;
  padding-left: 60px;
}

.messageList:hover{
  background-image: url("../assets/message/message-highlight.png");
}

.banghuiModle{
  margin: 0 auto;
  background-image: url('../assets/banghui/background.png');
  width: 1040px;
  height: 643px;
}
#banghuiBlock{
  display: flex;
  flex-wrap: wrap;
  justify-content: left;
  overflow: scroll;
  max-height: 400px;
  margin-top: 50px;
  margin-left: 80px;
  max-width: 900px;
}
.musicModle{
  margin:0 auto;
  height: 500px;
  width: 791px;
  background-image: url('../assets/music/background.png');
}
</style>
