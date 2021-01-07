<template>
    <canvas ref="canv" :width="width" :height="height"></canvas>
</template>

<style scoped>
  canvas{
    background-color: white;
  }
</style>

<script>
export default { 
  props: ["width","height","max","amount"],
  data() {
    return {
      triangle_bar_height_ratio: 0.6,
      flame_weight_ratio: 0.03, //キャンバスの横幅と高さの平均に対するボタンの枠の太さの割合
      line_ratio: 0.5, //三角ボタンの高さに対する＋の線の長さの割合
      line_weight_ratio: 0.8, //ボタンの枠の太さに対する＋,-の線の太さ
    }
  },
  methods: {
    draw_plus_btn(plus_color){
      //＋ボタンの色
      this.context.clearRect(0,0,this.width,this.height);
      this.context.beginPath();
      this.context.moveTo(this.width*0.5,this.flame_weight); 
      this.context.lineTo(this.flame_weight,this.height-this.flame_weight); 
      this.context.lineTo(this.width-this.flame_weight,this.height-this.flame_weight);
      this.context.closePath();	
      this.context.fillStyle = plus_color;
      this.context.fill();
      
      //＋ボタンの枠
      this.context.lineWidth = this.flame_weight;
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();

      //＋ボタンの"＋"の横棒
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.fillStyle= "rgba(0,0,0,1)";
      this.context.fillRect(this.width*0.5-this.line_width*0.5,this.triangle_bar_height,this.line_width,this.line_weight);
      //＋ボタンの"＋"の縦棒
      this.context.fillRect(this.width*0.5-this.line_weight*0.5,this.triangle_bar_height+this.line_weight*0.5-this.line_width*0.5,this.line_weight,this.line_width);
    },
  },
  mounted() {
    this.flame_weight = (this.width+this.height)*0.5*this.flame_weight_ratio //ボタンの枠の太さ
    this.triangle_bar_height = this.height*this.triangle_bar_height_ratio;
    this.line_width = this.height*this.line_ratio; //＋,-の線の長さ
    this.line_weight = this.flame_weight*this.line_weight_ratio; //＋,-の線の太さ

    let canvas = this.$refs.canv;
    this.context = canvas.getContext('2d');
   
    //+ボタンを作成
    let plus_color; //＋ボタンの色
    this.gradient = this.context.createLinearGradient(this.width*0.5,this.flame_weight,this.width*0.5,this.height-this.flame_weight);
    this.gradient.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    this.gradient.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    plus_color = this.gradient;
    this.draw_plus_btn(plus_color); 
    let touch = false;   

    
    //タッチされたとき（スマホ）
    canvas.addEventListener('touchstart', () => { 
        if (this.max === undefined ? true : this.amount<this.max){
          this.$emit("plusclick");
        } 
        plus_color = "rgba(0,0,0,0.4)";
        this.draw_plus_btn(plus_color);
        touch = true;
    }, {
      passive: true
    });

    //タッチが離れたとき（スマホ）
    canvas.ontouchend = (e) => {
        plus_color = this.gradient;
        this.draw_plus_btn(plus_color);
        touch = true;
    }

    //クリックが押されたとき
    canvas.onmousedown = (e) => {
      if (touch == false) {
        if (this.max === undefined ? true : this.amount<this.max){
          this.$emit("plusclick");
        } 
        plus_color = "rgba(0,0,0,0.4)";
        this.draw_plus_btn(plus_color); 
      }  
    }

    //クリックが離されたとき
    canvas.onmouseup = (e) => {
      if (touch == false) {
        plus_color = this.gradient;
        this.draw_plus_btn(plus_color);
      }
    }

    //マウスがキャンバス外に出た時
    canvas.onmouseout = (e) => {
      if (touch == false) {
        plus_color = this.gradient;
        this.draw_plus_btn(plus_color);
      }
    }                 
  }
}
</script>