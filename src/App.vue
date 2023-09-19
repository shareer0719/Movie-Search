<template>
  <form @submit.prevent :class="textCenter">
    <label>電影名稱:</label>
    <input v-model="movieTitle">
    <button @click="movieData">搜尋</button>
  </form><br>
    <h2 :class="textCenter">電影名稱:{{movieName}}</h2>
  <div :class="bigBox">
    <img :src="moviePoster">
    <div :class="box">
      <p>簡介:{{moviePlot}}</p>
      <p>導演:{{movieDirector}}</p>
      <p>演員:{{movieActors}}</p>
      <p>上映日:{{movieReleased}}</p>
    </div>
  </div>
  
</template>

<style>
  .textCenter {
    text-align: center;
  }
  .box {
    padding: 20px;
    /* color: white; */
  }
  .bigBox {
    background-color: lightgoldenrodyellow;
    display: flex;
    justify-content: center;
    padding: 20px;
  }
  
</style>

<script>
  import { ref } from 'vue';

  export default {
    setup() {
      // 這邊把API回傳json資料中需要使用的資料丟到ref裡
      const movieTitle = ref('');
      const moviePoster = ref('');
      const movieName = ref('');
      const movieDirector = ref('');
      const movieActors = ref('');
      const movieReleased = ref('');
      const movieYear = ref('');
      const moviePlot = ref('');
      
      //宣告函式movieData以fetch方式取得API傳來的資料,再用try,catch把成功或是失敗的結果紀錄
      const movieData = async () => {
        try {
          //宣告變數response儲存回傳的資料
          const response = await fetch(`https://www.omdbapi.com/?i=tt3896198&apikey=8215ec8c&t=${movieTitle.value}`);
          if (!response.ok) {
            throw new Error("fetch失敗");
          }
          //宣告變數result 把response的資料轉成json格式
          const result = await response.json();
          console.log(result);
          //加.value存需要的資料
          moviePoster.value = result.Poster
          movieName.value = result.Title
          movieDirector.value = result.Director
          movieActors.value = result.Actors
          movieReleased.value = result.Released
          movieYear.value = result.Year
          moviePlot.value = result.Plot
        } catch (error) {
          //console API連結有沒有錯誤
          console.error(error);
          throw error;
        }
      }
      const textCenter = ref('textCenter')
      const box = ref('box')
      const bigBox = ref('bigBox')
      
      return {
        movieTitle,
        movieData,
        moviePoster,
        movieName,
        movieDirector,
        movieActors,
        movieReleased,
        movieYear,
        moviePlot,
        textCenter,
        box,
        bigBox
      };
    },
  };
</script>