# plus_minus_button
‣example

----index.html----
````
<div id="App">
      <mycomp_plus
      :width = 500
      :height = 300
      :max = max
      :amount = amount
      :bg_c_1 = bg_c_1
      :bg_c_2 = bg_c_2
      @plusclick=amount++></mycomp_plus>        //@plusclick: ボタンが押されたらamountを1増やす
      <mycomp_minus
      :width = 500
      :height = 300
      :min = min
      :amount = amount
      :bg_c_2 = bg_c_2
      :bg_c_3 = bg_c_3
      @minusclick=amount--></mycomp_minus>      //@minusclick: ボタンが押されたらamountを1減らす
      <mycomp_num
      :width = 500
      :height = 300
      :amount = amount
      :bg_c_1 = bg_c_1
      :bg_c_2 = bg_c_2
      :bg_c_3 = bg_c_3
      @numclick="num"></mycomp_num>             //@numclick: ボタンが押されたらnum()発火
</div>
  ````
  
  ----index.js----
  ````
import Vue from 'vue';
import {component_plus, component_minus, component_num} from 'plus_minus_button';

new Vue({
    el: '#App',
    data: {
         max: 99,                               //最大値
         min: 0,                                //最小値
         amount: 20,                            //初期値
         bg_c_1: "rgba(100,99,255,0.05)",       //背景のグラデーション上部
         bg_c_2: "rgba(100,99,255,0.125)",      //背景のグラデーション中部
         bg_c_3: "rgba(100,99,255,0.25)",       //背景のグラデーション下部
    },
    components: {
        'mycomp_plus': component_plus,
        'mycomp_minus': component_minus,
        'mycomp_num': component_num,
    },　
    methods: {
        num(){
            console.log(this.amount);           //コンソールにamountを表示
        }
    }
})
````
