<template>
  <v-container>
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
        <v-divider></v-divider>
        <v-list-item class="pa-0">
          <v-list-item-content>
            <form>
              <v-list-item-title><h1>Connexion</h1></v-list-item-title>
              <v-row>
                <v-col cols="12">
                  <v-text-field v-model="email" label="Email" outlined dense hide-details/>
                </v-col>
                <v-col cols="12">
                  <v-text-field label="Password" v-model="password" outlined dense hide-details/>
                </v-col>
                <v-col cols="12" v-if="!isLogged">
                  <v-btn small outlined class="text-right">Connexion</v-btn>
                </v-col>
                <v-col cols="12" v-if="isLogged">
                  <v-btn small outlined class="text-right">Deconnexion</v-btn>
                </v-col>
              </v-row>
            </form>
            </v-list-item-content>
        </v-list-item>
        <v-divider></v-divider>
        <v-list-item class="px-0">
          <v-list-item-content>
            <v-list-item-title><h1>Inscription</h1></v-list-item-title>
            <v-row>
              <v-col cols="9">
                <v-text-field v-model="newEmail" label="Email" outlined dense hide-details/>
              </v-col>
              <v-col cols="9">
                <v-text-field label="Password" v-model="newPassword" outlined dense hide-details/>
              </v-col>
              <v-col cols="9">
                <v-select :items="filiereItem" label="Ma filiere" outlined/>
              </v-col>
              <v-col cols="9">
                <p>Ma signature :</p>
                <canvas ref="canvas" id="drawing-pad" class="white"></canvas>
              </v-col>
              <v-col cols="5">
                <v-btn small outlined @click="resetCanvas">Annuler</v-btn>
              </v-col>
              <v-col cols="5">
                <v-btn small outlined @click="saveImage">Signup</v-btn>
              </v-col>
            </v-row>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-container>
</template>

<script>
    export default {
      name: 'Menu',
      data () {
        return {
          email: '',
          password: '',
          newEmail: '',
          newPassword: '',
          isLogged: false,
          canvas: null,
          context: null,
          isDrawing: false,
          startX: 0,
          startY: 0,
          points: [],
          filiereItem: [
            'B1-Informatique',
            'B2-Informatique',
            'B3-Informatique',
            'B1-Audiovisuel',
            'B2-Audiovisuel',
            'B3-Audiovisuel',
            'B1-Communication-Marketing',
            'B2-Communication-Marketing',
            'B3-Communication-Marketing',
            'B1-Animation-3D-Jeux-Video',
            'B2-Animation-3D-Jeux-Video',
            'B3-Animation-3D-Jeux-Video',
            'M1-Data-Scientist',
            'M2-Data-Scientist',
            'M1-Expert-Cloud-Securite',
            'M2-Expert-Cloud-Securite',
            'M1-Expert-Dev-Web',
            'M2-Expert-Dev-Web',
            'M1-Expert-Dev-Iot',
            'M2-Expert-Dev-Iot',
            'M1-Entreprenariat-Start-Up',
            'M2-Entreprenariat-Start-Up',
            'M1-Marketing-Manager',
            'M2-Marketing-Manager',
            'M1-Strategie-Communication',
            'M2-Strategie-Communication',
            'M1-Directeur-Artistique',
            'M2-Directeur-Artistique',
          ],
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
          this.newEmail = '';
          this.newPassword= '';
          this.canvas.width = this.$refs.canvas.width;
          this.points.length = 0
        },

        saveImage(){
          let dataUrl = this.canvas.toDataURL();
          console.log(dataUrl);
        }
      }
    }
</script>

<style scoped>

</style>
