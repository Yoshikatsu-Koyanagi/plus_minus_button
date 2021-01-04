<template>
    <canvas ref="canv" :width="width" :height="height"></canvas>
</template>

<style scoped>

</style>

<script>
export default { 
  props: ["width","height","min","amount","bg_c_2","bg_c_3"],
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
    draw_minus_btn(minus_color){
      //-ボタンの枠
      this.context.globalCompositeOperation = "destination-out";
      this.context.beginPath();
      this.context.moveTo(this.tbw*0.5,this.triangle_top_margin+this.triangle_height); 
      this.context.lineTo(this.triangle_side_margin,this.triangle_top_margin); 
      this.context.lineTo(this.triangle_side_margin+this.triangle_width,this.triangle_top_margin);
      this.context.closePath();	
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fill();

      this.context.globalCompositeOperation = "source-over";
      this.context.lineWidth = this.flame_weight;
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      
      this.context.fillStyle = minus_color;
      this.context.fill();
   
      //-ボタンの"-"
      this.context.strokeStyle = "rgb(0,0,0)"; 
      this.context.stroke();
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.fillRect(this.tbw*0.5-this.line_width*0.5,this.triangle_top_margin+this.triangle_height-this.triangle_bar_height,this.line_width,this.line_weight);
  
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



    let b = this.$refs.canv;
    this.context = b.getContext('2d');
  


    this.gradient_b = this.context.createLinearGradient(this.tbw*0.5, 0, this.tbw*0.5, this.tbh);
    this.gradient_b.addColorStop(0.0 , this.bg_c_2);
    this.gradient_b.addColorStop(1.0 , this.bg_c_3);
    this.context.fillStyle = this.gradient_b;
    this.context.fillRect(0,0,this.tbw,this.tbh);

    
    //-ボタンを作成
    let minus_color; //-ボタンの色
    this.gradient = this.context.createLinearGradient(this.tbw*0.5,this.triangle_top_margin,this.tbw*0.5,this.triangle_top_margin+this.triangle_height);
    this.gradient.addColorStop(0.0, 'rgba(0,0,0,0.05)');
    this.gradient.addColorStop(0.8 , 'rgba(0,0,0,0.35)');
    minus_color = this.gradient 
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
        minus_color = this.gradient;
        this.draw_minus_btn(minus_color); 
    }

   
    b.onmouseout = (e) => {
        minus_color = this.gradient;
        this.draw_minus_btn(minus_color);
    } 
    
  }
}
</script>