# plus_minus_button
‣example

----index.html----
````
<div id="App">
      <mycomp_plus
      :width = 500
      :height = 300
      @plusclick="plus"></mycomp_plus> //@plusclick: ボタンが押されたらplus()発火
      <mycomp_minus
      :width = 500
      :height = 300
      @minusclick="minus"></mycomp_minus> //@minusclick: ボタンが押されたらminus()発火
      <mycomp_num
      :width = 500
      :height = 300
      :number = 20
      @numclick="num"></mycomp_num> //@numclick: ボタンが押されたらnum()発火
</div>
  ````
  
  ----index.js----
  ````
import Vue from 'vue';
import {component_plus, component_minus, component_num} from 'plus_minus_button';

new Vue({
    el: '#App',
    components: {
        'mycomp_plus': component_plus
        'mycomp_minus': component_minus
        'mycomp_num': component_num
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
