<template>
  <div id="app">
    <Calc v-bind:title="message" v-on:result-event="appAction" />
    <hr>
    <div><table v-html="log"></table></div>
  </div>
</template>

<script>
import Calc from './components/Calc.vue'  //Calcをインポート

export default {
  name: 'app',
  components: {
    Calc
  },
  data:function () {
    return {
      message: 'CALC',
      result: [],
    };
  },
  computed: {  // 算出プロパティ
    log () {
      var table = '<tr><th class="head">Expression</th><th class="head">Value</th></tr>';  //テーブル作成
      for(var i in this.result) {
        // テーブルにresultのi番目の配列の０番目の値とi番目の配列の１番目の値を入れる
        table += '<tr><td>' + this.result[i][0] + '</td><td>' + this.result[i][1] + '</td></tr>' ;  
      }
      return table;
    }
  },
  created: function(){
    var items = localStorage.getItem('log');  //Webブラウザに値を保存．getItemでlogキーを指定
    var logs = JSON.parse(items);
    if(logs != null) { this.result = logs;}
  },
  methods: {  // 初期化処理
    appAction (expre, res) {
      this.result.unshift([expre, res]);  //expreとresを配列の一番最初に追加
      if(this.result.length > 10) {  //10個を超えると最後を消す
        this.result.pop();
      }
      var log = JSON.stringify(this.result);  //JSON形式のテキストへ変換
      localStorage.setItem('log', log);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin: 5px;
}
tr td{
  padding: 5px;
  border: 1px solid gray;
}
tr th{
  padding: 5px;
  border: 5px;
}
tr th.head{
  background-color: black;
  color: white;
}
</style>
