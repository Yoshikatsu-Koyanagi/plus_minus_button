<template>
    <canvas ref="canv2" :width="width" :height="height"></canvas>
</template>

<style scoped>
 .canvas{
  }
</style>

<script>
export default { 
  props: ["width","height","min","amount"],
    data() {
      return {
        triangle_width_ratio: 0.8,
        triangle_height_ratio: 0.6,
        triangle_bar_height_ratio: 0.6,
        flame_weight_ratio: 0.01, //キャンバスの横幅に対するボタンの枠の太さの割合
        line_ratio: 0.5, //三角ボタンの高さに対する＋,-の線の長さの割合
        line_weight_ratio: 0.8, //ボタンの枠の太さに対する＋,-の線の太さ
    }
  },
  methods: {
    draw_minus_btn(minus_color){
      //-ボタンの枠
      this.context2.globalCompositeOperation = "destination-out";
      this.context2.beginPath();
      this.context2.moveTo(this.tbw*0.5,this.triangle_top_margin+this.triangle_height); 
      this.context2.lineTo(this.triangle_side_margin,this.triangle_top_margin); 
      this.context2.lineTo(this.triangle_side_margin+this.triangle_width,this.triangle_top_margin);
      this.context2.closePath();	
      this.context2.fillStyle = "rgba(0,0,0,1)";
      this.context2.fill();

      this.context2.globalCompositeOperation = "source-over";
      this.context2.lineWidth = this.flame_weight;
      this.context2.strokeStyle = "rgb(0,0,0)"; 
      this.context2.stroke();
      
      this.context2.fillStyle = minus_color;
      this.context2.fill();
   
      //-ボタンの"-"
      this.context2.strokeStyle = "rgb(0,0,0)"; 
      this.context2.stroke();
      this.context2.fillStyle = "rgba(0,0,0,1)";
      this.context2.fillRect(this.tbw*0.5-this.line_width*0.5,this.triangle_top_margin+this.triangle_height-this.triangle_bar_height,this.line_width,this.line_weight);
  
    },
  
  },
  mounted() {
    this.flame_weight = this.width*this.flame_weight_ratio //ボタンの枠の太さ


    this.tbw = this.width;
    this.tbh = this.height;
    
    this.triangle_width = this.triangle_width_ratio*this.tbw;
    this.triangle_height = this.tbh*this.triangle_height_ratio;
    this.triangle_side_margin = this.tbw*(1-this.triangle_width_ratio)*0.5;
    this.triangle_top_margin = this.tbh*(1-this.triangle_height_ratio)*0.5;
    this.triangle_bar_height = this.triangle_height*this.triangle_bar_height_ratio;
    this.line_width = this.triangle_height*this.line_ratio; //＋,-の線の長さ
    this.line_weight = this.flame_weight*this.line_weight_ratio; //＋,-の線の太さ



    let b = this.$refs.canv2;
    this.context2 = b.getContext('2d');
  


    this.gradient2_b = this.context2.createLinearGradient(this.tbw*0.5, 0, this.tbw*0.5, this.tbh);
    this.gradient2_b.addColorStop(0.0 , 'rgba(0,50,255,0.125)');
    this.gradient2_b.addColorStop(1.0 , 'rgba(0,50,255,0.25)');
    this.context2.fillStyle = this.gradient2_b;
    this.context2.fillRect(0,0,this.tbw,this.tbh);

    
    //-ボタンを作成
    let minus_color; //-ボタンの色
    this.gradient2 = this.context2.createLinearGradient(this.tbw*0.5,this.triangle_top_margin,this.tbw*0.5,this.triangle_top_margin+this.triangle_height);
    this.gradient2.addColorStop(0.0, 'rgba(0,0,0,0.05)');
    this.gradient2.addColorStop(0.8 , 'rgba(0,0,0,0.35)');
    minus_color = this.gradient2 
    this.draw_minus_btn(minus_color);


    b.onmousedown = (e) => {
        if (this.amount>this.min){
            this.$emit("minusclick");
        } 
        else {
        }
        minus_color = "rgba(0,0,0,0.4)";
        this.draw_minus_btn(minus_color);
    }
    

    b.onmouseup = (e) => {
        minus_color = this.gradient2;
        this.draw_minus_btn(minus_color); 
    }

   
    b.onmouseout = (e) => {
        minus_color = this.gradient2;
        this.draw_minus_btn(minus_color);
    } 
    
  }
}
</script>