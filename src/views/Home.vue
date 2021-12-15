<template>
  <div class="container">
    <div class="title">
      <h1>Typing Game</h1>
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

      <button class="backBtn" v-if="clearFlg" @click="init">戻る</button>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, watch, computed, nextTick } from "vue";

export default {
  setup() {
    //ゲームが開始しているかのフラグ
    let startFlg = ref(false);

    /**
     * ゲーム開始.
     */
    const gameStart = () => {
      //スタートしているかのフラグ
      startFlg.value = true;
      console.log(startFlg.value);
      //startボタンを押すとinputにフォーカスする。
      nextTick(() => {
        document.getElementById("typeForm").focus();
      });
    };
    //現在の問題
    const currentQuestion = ref("");
    //問題集
    const questions = ref([
      "onclick",
      "onchange",
      "ondblclick",
      "ondrag",
      "onkeypress",
      "onload",
      "onmouseover",
      "onscroll",
      "onselect",
      "toLocaleString",
    ]);

    /**
     * 問題集の配列を順番に表示.
     */
    onMounted(() => {
      currentQuestion.value = questions.value[0];
      questionCount.value = questions.value.length;
    });

    /**
     * 配列を並び替える処理.
     */
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    const arrayShuffle = (array) => {
      for (let i = array.length - 1; 0 < i; i--) {
        // 0〜(i+1)の範囲で値を取得
        const r = Math.floor(Math.random() * (i + 1));

        // 要素の並び替えを実行
        const tmp = array[i];
        array[i] = array[r];
        array[r] = tmp;
      }
      return array;
    };

    //問題の配列の並び替え
    console.log(arrayShuffle(questions.value));

    //入力欄
    const typeBox = ref("");

    /**
     * 答え合わせ.
     *
     * @remarks 入力された文字と問題を比較して正解を確認。
     *
     */
    watch(typeBox, (input) => {
      if (input == currentQuestion.value) {
        //問題の配列から正解した問題を削除
        questions.value.splice(0, 1);
        currentQuestion.value = questions.value[0];
        //入力欄を空にする
        typeBox.value = "";
        //正解数の表示を１プラス
        currentQuestionCount.value = currentQuestionCount.value + 1;
      }
    });
    //現在の問題番号
    let currentQuestionCount = ref(0);
    //全問題数
    let questionCount = ref(0);

    //クリアしたかのフラグ
    const clearFlg = ref(false);
    //ゲージの増える量を調整する。
    const gaugeAdjuster = ref(0);
    //ゲージが増える処理
    const styleObject = computed(() => {
      //変数width_valueに(100% / 問題数 + 減ってしまった分を足す変数i)代入
      const widthValue = 100 / (questions.value.length + gaugeAdjuster.value);
      const width = widthValue * currentQuestionCount.value + "%";
      let color = "";
      if (
        currentQuestionCount.value ==
        questions.value.length + gaugeAdjuster.value
      ) {
        //全問正解
        color = "rgb(255, 123, 0";
        //クリアしたら戻るボタンを表示するためのフラグ
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        clearFlg.value = true;
      } else {
        //回答中
        color = " #FFA07A";
      }
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      gaugeAdjuster.value++;
      return {
        width: width,
        "background-color": color,
      };
    });

    /**
     * 更新処理.
     */
    const init = () => {
      location.reload();
    };

    return {
      startFlg,
      gameStart,
      currentQuestion,
      questions,
      onMounted,
      typeBox,
      currentQuestionCount,
      questionCount,
      styleObject,
      clearFlg,
      init,
      arrayShuffle,
    };
  },
};
</script>
