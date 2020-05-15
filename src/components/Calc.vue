<template>
    <div class="hello">
        <h1>{{title}}</h1>
        <p>{{message}}</p>
        <hr>
        <div>
            <div><textarea v-model="fomula" cols="40" rows="5"></textarea></div>
            <div><button v-on:click="doAction">CALC！</button></div>  <!-- ボタンを押すとdoActionが発火 -->
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Calc',  //Calcとしてexport
        props: {
            title: String,
        },
        data () {
            return {
                message: 'Enter expression:',  //初期表示
                fomula: '0',
            };
        },
        methods:{
            doAction () {
                var arr = this.fomula.trim().split('\n');  //arrにfomulaの前後の空白を取り除き配列にする
                var last = arr.pop();  //arrの末尾を取得
                var fn = '';           //空白
                for(var n in arr) {
                    if(arr[n].trim() != '') {          //もし空白でなければ
                        fn += 'var ' + arr[n] + ';';   //arrのn番目の値をfnに足す
                    }
                }
                fn += 'return '  +  last + ';';        //fn+lastをreturn
                var exp='function f(){' + fn +  '} f();';    // fという関数にfnで完成したのテキストをまとめて実行できるようにする
                var ans = eval(exp);                 //expをevalする
                this.message = 'answer: ' + ans;       // 実行した結果を
                var res = arr.join(';').trim();     // resultにarrに；を足したものを代入
                if (res != '') { res += ';'}     // resultが空白でなければ；をつけてlastを追加
                res += last;
                this.$emit('result-event', res, ans);    // resultとansを引数に指定し，result-eventのイベントを呼び出す
            }
        }
    }
</script>