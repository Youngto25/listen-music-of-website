<template>
  <div>
    <header></header>
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
            <svg class="icon" v-if="seePlay" @click="playSong">
              <use xlink:href="#icon-play" />
            </svg>
            <svg class="icon" v-else @click="pauseSong">
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
          console.log(this.results)
        });
    },
    playSong(event){
      this.seePlay = false
      this.url = event.target.parentNode.getAttribute('data-url')
      setTimeout(()=>{
        this.$el.querySelector("#audio").play()
      },0)
    },
    pauseSong(event){
      this.seePlay = true
      this.$el.querySelector("#audio").pause()
    }
  }
};
</script>
<style  lang="scss" scoped>
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
    width: 956px;
    position: relative;
    left: 50%;
    transform: translateX(-50%);
    .title{
      background: #F7F7F7;
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
      }
      &:hover{
        background: #e6f8f9;
        .icon{
          visibility: visible;
        }
      }
      span{
        display: inline-block;
        height: 36px;
        line-height: 36px;
        font-size: 14px;
        &:first-child{
          padding-left: 16px;
          width: 400px;
          color: #31c27c;
        }
      }
    }
    
  }
}
</style>
