<template>
  <v-app dark>
    <v-content>
      <v-navigation-drawer
        app
        fill-height
        floating
        dark
      >
        <v-list
          dense
          nav
          class="py-0"
        >
          <v-list-item two-line class="px-0">
            <v-list-item-content>
              <v-list-item-title>Chuuuuut</v-list-item-title>
              <v-list-item-subtitle>Accès pour Ynov</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
          <v-divider v-if="!isLogged"></v-divider>
          <v-list-item v-if="!isLogged" class="pa-0">
            <v-list-item-content>
              <v-row>
                <v-col cols="12">
                  <v-text-field v-model="email" label="Email" outlined dense hide-details/>
                </v-col>
                <v-col cols="12">
                  <v-text-field label="Password" v-model="password" outlined dense hide-details/>
                </v-col>
                <v-col cols="6" >
                  <v-btn small outlined>Inscription</v-btn>
                </v-col>
                <v-col cols="6" >
                  <v-btn small outlined>connexion</v-btn>
                </v-col>
              </v-row>
            </v-list-item-content>
          </v-list-item>
          <v-divider></v-divider>
          <v-list-item v-if="!isLogged" class="px-0">
            <v-list-item-content>
              <v-list-item-title><h1>Inscription</h1></v-list-item-title>
              <v-row dense>
                <v-col cols="9">
                  <v-text-field v-model="email" label="Email" outlined dense hide-details/>
                </v-col>
                <v-col cols="9">
                  <v-text-field label="Password" v-model="password" outlined dense hide-details/>
                </v-col>
                <v-col cols="9">
                  <canvas ref="canvas" id="drawing-pad" class="white"></canvas>
                </v-col>
                <v-col cols="5">
                <v-btn small outlined @click="saveImage">S'inscrire</v-btn>
              </v-col><v-col cols="5">
                <v-btn small outlined @click="resetCanvas">Annuler</v-btn>
              </v-col>
              </v-row>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
      <img ref="img" src="" id="canvas-img">
      <nuxt />
    </v-content>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        email: '',
        password: '',
        isLogged: false,
        logInToggle: false,
        singIn:true,
        canvas: null,
        context: null,
        isDrawing: false,
        startX: 0,
        startY: 0,
        points: [],
      }
    },
    mounted() {

      this.canvas = this.$refs.canvas;
      this.context = this.canvas.getContext('2d');
      this.canvas.addEventListener('mousedown',this.mouseDown);
      this.canvas.addEventListener('mousemove',this.mouseMove);
      document.addEventListener('mouseup',this.mouseUp);
    },
    methods: {
      mouseDown(e) {
        let rect = this.canvas.getBoundingClientRect();
        let x = e.clientX - rect.left;
        let y = e.clientY - rect.top;

        this.isDrawing = true;
        this.startX = x;
        this.startY = y;
        this.points.push({
          x: x,
          y: y,
        });
      },

      mouseMove(e) {
        let rect = this.canvas.getBoundingClientRect();
        let x = e.clientX - rect.left;
        let y = e.clientY - rect.top;

        if (this.isDrawing){
          this.context.beginPath();
          this.context.moveTo(this.startX,this.startY);
          this.context.lineTo(x, y);
          this.context.lineWidth = 1;
          this.context.lineCap = 'round';
          this.context.strokeStyle = "rgba(0,0,0,1)";
          this.context.stroke();

          this.startX = x;
          this.startY = y;

          this.points.push({
            x: x,
            y: y,
          });
        }
      },

      mouseUp() {
        this.isDrawing = false;
        if (this.points.length > 0 ) {
          localStorage['points'] = JSON.stringify(this.points);
        }
      },

      resetCanvas(){
        this.canvas.width = this.$refs.canvas.width;
       this.points.length = 0
      },

      saveImage(){
        let dataUrl = this.canvas.toDataURL();
        let img = this.$refs.img;
        img.src = dataUrl;
      }
    }
  }
</script>
