<template>
    <canvas ref="canv" :width="width" :height="height"></canvas>
</template>

<style scoped>

</style>

<script>
export default { 
  props: ["width","height","max","amount","bg_c_1","bg_c_2"],
    data() {
      return {
        triangle_width_ratio: 0.9,
        triangle_height_ratio: 0.7,
        triangle_bar_height_ratio: 0.6,
        flame_weight_ratio: 0.01, //キャンバスの横幅に対するボタンの枠の太さの割合
        line_ratio: 0.5, //三角ボタンの高さに対する＋,-の線の長さの割合
        line_weight_ratio: 0.8, //ボタンの枠の太さに対する＋,-の線の太さ
      }
  },
  methods: {
    draw_plus_btn(plus_color){
      //＋ボタンの枠
      this.context.globalCompositeOperation = "destination-out";
      this.context.beginPath();
      this.context.moveTo(this.tbw*0.5,this.triangle_top_margin); 
      this.context.lineTo(this.triangle_side_margin,this.triangle_top_margin+this.triangle_height); 
      this.context.lineTo(this.triangle_side_margin+this.triangle_width,this.triangle_top_margin+this.triangle_height);
      this.context.closePath();	
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fill();
      
      this.context.globalCompositeOperation = "source-over";
      this.context.lineWidth = this.flame_weight;
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle = plus_color;
      this.context.fill();
      
      //＋ボタンの"＋"の横棒
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.fillStyle= "rgba(0,0,0,1)";
      this.context.fillRect(this.tbw*0.5-this.line_width*0.5,this.triangle_top_margin+this.triangle_bar_height,this.line_width,this.line_weight);

      //＋ボタンの"＋"の縦棒
      this.context.fillRect(this.tbw*0.5-this.line_weight*0.5,this.triangle_top_margin+this.triangle_bar_height+this.line_weight*0.5-this.line_width*0.5,this.line_weight,this.line_width);
    },

  },
  mounted() {
    this.tbw = this.width;
    this.tbh = this.height;

    this.flame_weight = this.tbw*this.flame_weight_ratio //ボタンの枠の太さ
    this.triangle_width = this.triangle_width_ratio*this.tbw;
    this.triangle_height = this.tbh*this.triangle_height_ratio;
    this.triangle_side_margin = this.tbw*(1-this.triangle_width_ratio)*0.5;
    this.triangle_top_margin = this.tbh*(1-this.triangle_height_ratio)*0.5;
    this.triangle_bar_height = this.triangle_height*this.triangle_bar_height_ratio;
    this.line_width = this.triangle_height*this.line_ratio; //＋,-の線の長さ
    this.line_weight = this.flame_weight*this.line_weight_ratio; //＋,-の線の太さ

    let canvas = this.$refs.canv;
    this.context = canvas.getContext('2d');

    if (this.bg_c_1 === undefined){
      this.bg1 = "rgba(255,99,255,0.05)";
    } 
    if (this.bg_c_2 === undefined){
      this.bg2 = "rgba(255,99,255,0.125)";
    }
    
    this.gradient_b = this.context.createLinearGradient(this.tbw*0.5, 0, this.tbw*0.5, this.tbh);
    this.gradient_b.addColorStop(0.0 , this.bg1);
    this.gradient_b.addColorStop(1.0 , this.bg2);
    this.context.fillStyle = this.gradient_b;
    this.context.fillRect(0,0,this.tbw,this.tbh);

   
    //+ボタンを作成
    let plus_color; //＋ボタンの色
    this.gradient = this.context.createLinearGradient(this.tbw*0.5,this.triangle_top_margin,this.tbw*0.5,this.triangle_top_margin+this.triangle_height);
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