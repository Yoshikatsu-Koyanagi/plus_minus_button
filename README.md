# plus_minus_button
‣example

----index.html----
````
<div id="App">
      <mycomp
      :width = 500
      :height = 300
      @plusclick="plus"></mycomp> //@plusclick: ボタンが押されたらplus()発火
      <mycomp2
      :width = 500
      :height = 300
      @minusclick="minus"></mycomp2> //@minusclick: ボタンが押されたらminus()発火
      <mycomp3
      :width = 500
      :height = 300
      :number = 20
      @numclick="num"></mycomp3> //@numclick: ボタンが押されたら発火
</div>
  ````
  
  ----index.js----
  ````
import Vue from 'vue';
import {component1, component2, component3} from 'plus_minus_button';

new Vue({
    el: '#App',
    components: {
        'mycomp1': component1
        'mycomp2': component2
        'mycomp3': component3
    },
    methods: {
        plus(){
            console.log("plus")
        },
        minus(){
            console.log("minus")
        },
        num(){
            console.log("num")
        }
    }
})
````
