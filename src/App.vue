<template>
  <div>
    <header>
      <div class="logo">
        Fmusic
      </div>
    </header>
    <main>
      <div class="search-wrapper">
        <div class="search">
          <input v-model="songName" />
          <svg class="icon" @click="getSongList">
            <use xlink:href="#icon-search" />
          </svg>
        </div>
      </div>
      <div class="songList">
        <ul v-if="show">
          <div class="title"><span>歌曲</span><span>歌手</span></div>
          <li v-for="(result,index) in results" :key="index" :data-url="result.url">
            <span>{{result.title}}</span>
            <svg class="icon play" @click="playSong">
              <use xlink:href="#icon-play" />
            </svg>
            <svg class="icon pause" @click="pauseSong" style="display: none;">
              <use xlink:href="#icon-pause" />
            </svg>
            <span>{{result.author}}</span>
          </li>
        </ul>
      </div>
    </main>
    <audio muted autoplay loop :src="url" id="audio"></audio>
  </div>
</template>
<script>
import "./svg/svg.js";
export default {
  data() {
    return {
      results: [],
      songName: "真心英雄",
      show: false,
      seePlay: true,
      url: ''
    };
  },
  mounted() {
    this.getSongList()
  },
  methods: {
    getSongList() {
      this.$http
        .get(`https://api.apiopen.top/searchMusic?name=${this.songName}`)
        .then(response => {
          this.results = response.data.result;
          this.show = true;
          this.modifyIcon()
        });
    },
    playSong(event){
      this.modifyIcon()
      var parent = event.target.parentNode
      this.url = parent.getAttribute('data-url')
      parent.children[1].style.display = 'none'
      parent.children[2].style.display = 'block'
      setTimeout(()=>{
        this.$el.querySelector("#audio").play()
      },0)
    },
    pauseSong(event){
      var parent = event.target.parentNode
      parent.children[1].style.display = 'block'
      parent.children[2].style.display = 'none'
      this.$el.querySelector("#audio").pause()
    },
    modifyIcon(){
      let playIcon = this.$el.querySelectorAll('.play')
      let pauseIcon = this.$el.querySelectorAll('.pause')
      for(let i = 0; i < playIcon.length; i++){
        playIcon[i].style.display = 'block'
        pauseIcon[i].style.display = 'none'
      }
    }
  }
};
</script>
<style  lang="scss" scoped>
 @font-face {
    font-family: 'logo';
    src: url('font/King Lionel - Personal Use.ttf')  format('truetype');
}
 @font-face {
    font-family: 'my-font';
    src: url('font/Ming Imperial.TTF')  format('truetype');
}
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
ul,ol{
  list-style: none;
}
header {
  background: #fff;
  height: 90px;
  display: flex;
  align-items: center;
  .logo{
    font-size: 48px;
    margin-left: 48px;
    color: #31c27c;
    font-family: 'logo';
  }
}
main {
  .search-wrapper {
    background: #107f63;
    height: 250px;
    display: flex;
    justify-content: center;
    align-items: center;
    .search {
      border: 1px solid #ccc;
      display: flex;
      align-items: center;
      border-radius: 4px;
      input {
        border: none;
        height: 48px;
        width: 544px;
        padding: 10px;
        font-size: 16px;
        outline: none;
      }
      .icon{
        width: 40px;
        height: 48px;
        padding: 2px;
        fill: #111;
        background: #fff;
        &:hover{
          cursor: pointer;
        }
      }
    }
    &:focus{
      border-color: #111;
    }
  }
  .songList{
    margin-top: 20px;
    width: 666px;
    position: relative;
    left: 50%;
    transform: translateX(-50%);
    .title{
      background: #f1f2f6;
      span{
        display: inline-block;
        height: 36px;
        line-height: 36px;
        &:first-child{
          padding-left: 16px;
          width: 400px;
        }
      }
    }
  }
  ul{
    li{
      padding: 4px 0;
      display: flex;
      align-items: center;
      .icon{
        height: 40px;
        width: 40px;
        margin-right: 10px;
        position: absolute;
        left: 350px;
        visibility: hidden;
        cursor: pointer;
      }
      &:hover{
        background: #F7F7F7;
        .icon{
          visibility: visible;
          &:hover{
            fill: #31c27c;
          }
        }
      }
      span{
        display: inline-block;
        height: 36px;
        line-height: 36px;
        font-size: 14px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        &:first-child{
          padding-left: 16px;
          width: 400px;
          color: #31c27c;
        }
        &:last-child{
          width: 266px;
        }
      }
    }
    
  }
}
</style>
