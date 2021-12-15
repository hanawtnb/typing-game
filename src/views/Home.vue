<template>
  <div class="container">
    <div class="title">
      <h1>Typing Game</h1>
      <div class="marker"></div>
    </div>
    <div class="gameScreen">
      <button v-if="startFlg != true" class="startBtn mb-20" @click="gameStart">
        START!
      </button>
      <div class="questionWrapper" v-if="startFlg">
        <div class="question mb-20">{{ currentQuestion }}</div>
        <div class="clear" v-if="currentQuestionCount === questionCount">
          Clear!
        </div>
        <div class="typeFormWrapper mb-20">
          <input id="typeForm" type="text" class="typeForm" v-model="typeBox" />
        </div>
        <div class="gaugeWrapper mb-20">
          <div v-bind:style="styleObject" class="gauge"></div>
        </div>
        <div class="questionCount">
          {{ currentQuestionCount }}/{{ questionCount }}
        </div>
      </div>
      <br />

      <button v-if="clearFlg" @click="init">戻る</button>
      <!-- <input type="text" v-model="keyWord" />
      <button @click="getWords(keyWord)">search</button> -->
    </div>
  </div>
</template>

<style scoped>
* {
  font-family: "Fredoka One", cursive;
}
.mb-20 {
  margin-bottom: 20px;
}
.container {
  width: 400px;
  margin: 0 auto;
  text-align: center;
}

.title {
  position: relative;
  font-size: 30px;
}

.gameScreen {
  background-color: white;
  padding: 40px;
}

.startBtn {
  background-color: #87ceeb;
  color: white;
  padding: 4px 60px;
  border: none;
  outline: none;
  border-radius: 8px;
  cursor: pointer;
}

.startBtn:hover {
  opacity: 0.7;
}

.question {
  color: grey;
  font-size: 40px;
}

.clear {
  color: rgb(255, 123, 0);
  font-size: 50px;
}
.typeForm {
  text-align: center;
  outline: none;
  border: none;
  font-size: 30px;
}

.typeFormWrapper {
  border-bottom: 1px solid grey;
}

.gauge {
  height: 18px;
  transition: all 0.3s ease;
  border-radius: 20px;
}

.gaugeWrapper {
  border: 1px solid;
  height: 18px;
  border-radius: 20px;
  background-color: white;
}

.questionCount {
  font-size: 20px;
}
</style>
