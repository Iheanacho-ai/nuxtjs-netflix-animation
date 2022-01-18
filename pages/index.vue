<template>
  <div>
    <video 
    id= "video-player"
    class="cld-video-player cld-video-player-skin-dark w-500 h-300"
    autoplay
    >

    </video>
    <transition name="fade">
      <div class = 'overlay' v-if = 'overlay' style="animation-duration: 1s">
        <h2 class= 'overlay-header'>bonne vitesse</h2>
        <p>
          You look down on where you came from sometimes
          But you'll have this place to call home, always.
        </p>
        <button @click= "playMedia()">Play</button>
      </div>
    </transition> 

    
  </div>
</template>

<script>

require('vue2-animate/dist/vue2-animate.min.css')


export default {
  data(){
    return{
      cld: null,
      player: null,
      video: "production_ID_4456999_gr6iy4",
      overlay: true,
    }
  },
  methods: {
    playMedia(){
      this.player.play(),
      this.overlay = false
    }
  },
  mounted(){
    this.cld= cloudinary.Cloudinary.new({
      cloud_name:  process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME,
      secure:  true

    })

    this.player = this.cld.videoPlayer(
      'video-player',{
        autoplay: true,
        controls: "!this.overlay"
      }
    );
    this.player.source(this.video);

    this.player.on('ended', ()=> {
      this.overlay = true
    })

    setTimeout(() => {
      this.overlay = false
    }, 5000)
  }




}



</script>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300&display=swap');

div{
  margin: 0;
  padding: 0;
}
.cld-video-player{
  width: 500px;
  height: 300px;
}
.overlay{
  box-sizing: border-box;
  font-family: 'Nunito', sans-serif;
  width: 250px;
  height: 300px;
  padding: 10px;
  background-color: rgba(0, 0, 0, 0.35);
  color: #fff;
  position: absolute;
  top: 7px;
  left: 7px;
  bottom: 7px;
}
.overlay-header{
  text-align: center;
}
button{
  width: 90%;
  height: 30px;
  margin: 0 auto;
}

</style>