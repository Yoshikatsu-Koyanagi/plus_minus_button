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
  props: ["width","height","min","amount"],
  data() {
    return {
      triangle_bar_height_ratio: 0.6,
      flame_weight_ratio: 0.03, //キャンバスの横幅に対するボタンの枠の太さの割合
      line_ratio: 0.5, //三角ボタンの高さに対する-の線の長さの割合
      line_weight_ratio: 0.8, //ボタンの枠の太さに対する＋,-の線の太さ
    }
  },
  methods: {
    draw_minus_btn(minus_color){
      //-ボタンの色
      this.context.clearRect(0,0,this.width,this.height);
      this.context.beginPath();
      this.context.moveTo(this.width*0.5,this.height-this.flame_weight); 
      this.context.lineTo(this.flame_weight,this.flame_weight); 
      this.context.lineTo(this.width-this.flame_weight,this.flame_weight);
      this.context.closePath();	
      this.context.fillStyle = minus_color;
      this.context.fill();

      //-ボタンの枠
      this.context.lineWidth = this.flame_weight;
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      
      //-ボタンの"-"
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fillRect(this.width*0.5-this.line_width*0.5,this.height-this.triangle_bar_height-this.line_weight,this.line_width,this.line_weight);
  
    },
  },
  mounted() {
    this.flame_weight = (this.width+this.height)*0.5*this.flame_weight_ratio //ボタンの枠の太さ;
    this.triangle_bar_height = this.height*this.triangle_bar_height_ratio;
    this.line_width = this.height*this.line_ratio; //＋,-の線の長さ
    this.line_weight = this.flame_weight*this.line_weight_ratio; //＋,-の線の太さ

    let canvas = this.$refs.canv;
    this.context = canvas.getContext('2d');
    
    //-ボタンを作成
    let minus_color; //-ボタンの色
    this.gradient = this.context.createLinearGradient(this.width*0.5,this.flame_weight,this.width*0.5,this.height-this.flame_weight);
    this.gradient.addColorStop(0.0, 'rgba(0,0,0,0.05)');
    this.gradient.addColorStop(0.8 , 'rgba(0,0,0,0.35)');
    minus_color = this.gradient 
    this.draw_minus_btn(minus_color);
    let touch = false;    


    //タッチされたとき（スマホ）
    canvas.addEventListener('touchstart', () => { 
        if ( this.min===undefined ? true : this.amount>this.min ){
          this.$emit("minusclick");
        } 
        minus_color = "rgba(0,0,0,0.4)";
        this.draw_minus_btn(minus_color);
        touch = true;
    }, {
      passive: true
    });

    //タッチが離れたとき（スマホ）
    canvas.ontouchend = (e) => {
        minus_color = this.gradient;
        this.draw_minus_btn(minus_color);
        touch = true;
    }

    //クリックが押されたとき
    canvas.onmousedown = (e) => {
      if (touch == false) {
        if ( this.min===undefined ? true : this.amount>this.min ){
          this.$emit("minusclick");
        } 
        minus_color = "rgba(0,0,0,0.4)";
        this.draw_minus_btn(minus_color);
      }
    }

    //クリックが離されたとき
    canvas.onmouseup = (e) => {
      if (touch == false) {
        minus_color = this.gradient;
        this.draw_minus_btn(minus_color); 
      }
    }

    //マウスがキャンバス外に出た時
    canvas.onmouseout = (e) => {
      if (touch == false) {
        minus_color = this.gradient;
        this.draw_minus_btn(minus_color);
      }
    } 
  }
}
</script>