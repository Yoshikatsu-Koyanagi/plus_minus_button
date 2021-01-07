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
  props: ["width","height","amount"],
  data() {
    return {
      font_ratio: 0.8, //数字ボタンの高さに対するフォントサイズの割合
      font_name: "meirio", //フォントの書体
    }
  },
  methods: {
    draw_num_btn(num_color){
      this.context.clearRect(0,0,this.width,this.height);
      this.context.fillStyle = num_color;
      this.context.fillRect(0,0,this.width,this.height);

      //数字ボタンの"数字"
      this.context.font = this.font;
      this.context.fillStyle = "rgba(0,0,0,1)";
      this.context.textAlign = "center";
      this.context.fillText(this.amount, this.width*0.5, this.height*0.8,this.width);
    },
  },
  mounted() {   
    let canvas = this.$refs.canv;
    this.context = canvas.getContext('2d');
    
    //数字ボタンを作成
    this.font_size = this.height*this.font_ratio; //フォントサイズ
    this.font = `${this.font_size}px ${this.font_name}`; //フォントサイズと書体    
    
    this.gradient = this.context.createLinearGradient(this.width*0.5,0,this.width*0.5,this.height);
    this.gradient.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    this.gradient.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    this.gradient_pushed = this.context.createLinearGradient(this.width*0.5,0,this.width*0.5,this.height);
    this.gradient_pushed.addColorStop(0.0 , 'rgba(0,0,0,0.5)');
    this.gradient_pushed.addColorStop(0.8 , 'rgba(0,0,0,0.3)');

    let num_color; //数字ボタンの色
    num_color = this.gradient;
    this.draw_num_btn(num_color);

    let num_down = false;
    let touch = false;
    
    //タッチされたとき（スマホ）
    canvas.addEventListener('touchstart', () => { 
        num_color = this.gradient_pushed;
        this.draw_num_btn(num_color);
        num_down = true;
        touch = true;
    },　{
      passive: true
    });

    //タッチが離れたとき（スマホ）
    canvas.ontouchend = (e) => {
        num_color = this.gradient;
        this.draw_num_btn(num_color);  
        //数字ボタンが押されて且つ数字ボタン上で離されたとき        
        if (num_down == true) {
        this.$emit("numclick");
        num_down = false;
        touch = true;
        }
    }
    
    canvas.onmousedown = (e) => {
      if (touch == false) {
        num_color = this.gradient_pushed;
        this.draw_num_btn(num_color);
        num_down = true;
      }
    }

    //クリックが離されたとき
    canvas.onmouseup = (e) => {
      if (touch == false) {
        num_color = this.gradient;
        this.draw_num_btn(num_color); 
        //数字ボタンが押されて且つ数字ボタン上で離されたとき        
        if (num_down == true) {
          this.$emit("numclick");
          num_down = false;
          }
        }
      } 

    //マウスがキャンバス外に出た時
    canvas.onmouseout = (e) => {
      if (touch == false) {
        num_color = this.gradient;
        this.draw_num_btn(num_color);
        num_down = false;
      }
    }      
  },
  watch: {
    amount: function(){
      this.draw_num_btn(this.gradient)
    }
  }
}
</script>