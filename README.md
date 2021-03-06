# SimpleMusicPlayer

## 整体布局
![布局](./images/demo.png "demo")

- div_wrap 整个页面

    - div player 中间矩形
        - div serach bar
            - img
            - input
        - div center-con 主要内容部分
        - div audio-con 音乐播放栏
            - audio
        - div video 特殊情况下触发
            - video

- div center-con
    - div song
        - ul
        - img line
    - player-con
        - img bar
        - img disc
        - img cover
    - comment
        - h5
        - div
            - dl
        - img line

## 功能设计
- 搜索
- 播放音乐
- 播放视频
- 展示热评
- 动态效果

## 用到的Vue
- vue-on
``` js
<audio ref='audio' @play="play" @pause="pause" ...
// 动作绑定函数，如回车搜索，播放歌曲
```
- vue-bind
``` js
<audio ref='audio' :src="musicUrl" ...
// 例如src属性与 musicUrl绑定，设置元素， :src
```

- vue-model
``` js
<!-- 搜索歌曲 -->
        <input type="text" autocomplete="off" v-model='query' @keyup.enter="searchMusic();" />
// v-model用于双向绑定，只能用于表单，用于输入数据交互
```
- vue-for
``` js
<ul class="song_list">
            <li v-for="item in musicList">
              <!-- 点击放歌 -->
              <a href="javascript:;" @click='playMusic(item.id)'></a>
              <b>{{item.name}}</b>
// vue-for 用于循环展示数据，这里 <a 在css中渲染了点击播放图片
```

- vue-if vue-show
``` js
<i @click="playMv(item.mvid)" v-if="item.mvid!=0"></i>
// 播放mv按钮显示与否
```
- vue 对象判断
``` js
<div class="player_con" :class="{playing:isPlay}">
    <img src="images/player_bar.png" class="play_bar" />
    <!-- 黑胶碟片 -->
    <img src="images/disc.png" class="disc autoRotate" />
    <img :src="coverUrl==''?'./images/cover.png':coverUrl" class="cover autoRotate" />
</div>

// 通过isplay判断是否需要渲染动画，这里通过改变 isplay对象来判断
// 动态的本质是是否改变元素的类名，css中都有

/* 旋转的类名 */
.autoRotate {
  animation-name: Rotate;
  animation-iteration-count: infinite;
  animation-play-state: paused;
  animation-timing-function: linear;
  animation-duration: 5s;
}
/* 是否正在播放 */
.player_con.playing .disc,
.player_con.playing .cover {
  animation-play-state: running;
}

.disc {
  position: absolute;
  left: 73px;
  top: 60px;
  z-index: 9;
}
```

## 用到的网络通讯
``` js
var app = new Vue({
      el: "#player",
      data: {
        // 搜索关键字
        query: '',
        // 歌曲列表
        musicList: [],
        // 歌曲url
        musicUrl: '',....
    
    methods: {
    // 歌曲搜索
    searchMusic: function() {
      var that = this;  //axios会改变指向，最好用that指向vue对象
      axios.get("https://autumnfish.cn/search?keywords=" + this.query).then(
        function(response) {
          // console.log(response);
          that.musicList = response.data.result.songs;
          console.log(response.data.result.songs);
        },
        function(err) {}
      );
    },
        // 隐藏
    hide: function() {
      this.isShow = false;
    }，
    ...
  }
    ...
```