<template>
  <div>
    <p>{{ hhmmss }}</p>
    <button type="button"
           v-if="true"
           v-on:click="startClicked" class="btn btn-primary">START</button>
    <button type="button"
           v-if="true"
           v-on:click="stopClicked" class="btn btn-danger">STOP</button>
  </div>
</template>
<script>
  export default {
      props: {
          seconds: {
              type: Number,
              default: 10  
          }
      },
      watch: {
          isRunning: function(value) {
              console.log('Data isRunning is changed to: ' + value)

              var self = this

              if(value) {
                // Start the count down
                self.timerId = setInterval( function(){ 
                    console.log('Inside interval')
                    if(self.currentSeconds > 0) {
                        self.currentSeconds--; 
                    } else {
                        
                        self.isRunning = false
                    }
                }, 1000 )
              } else {
                if ( self.timerId != -1) {
                    self.stopTimer()
                }
              }
          }
      },
      /*
        props
        data
        filters
        watch
        mounted
        destroyed
        methods
        computed
      */
      mounted: function() {
        console.log('Timer mounted')
        // this.currentSeconds = this.seconds
      },
      data: function() {
        return {
            isRunning: false,
            currentSeconds: this.seconds,
            timerId: -1
        }
      },
      methods: {
        stopTimer: function(){
            var self = this;
            if(self.timerId != -1) clearInterval( self.timerId )
            self.timerId = -1            
        }, 
        pad: function(num, size) {
            var s = "00" + num;
            return s.substr(s.length-size);
        },
        startClicked: function(){
            console.log('Start clicked')
            this.isRunning = true;
        },
        stopClicked: function(){
            console.log('Stop clicked')
            this.isRunning = false;
        }
      },
      computed: {
          hhmmss: function() {
              var totalSeconds = this.currentSeconds;
              var hours = Math.floor(totalSeconds / 3600);
              totalSeconds %= 3600;
              var minutes = Math.floor(totalSeconds / 60);
              var seconds = totalSeconds % 60;

              
              return this.pad(hours,2) + ":" + this.pad(minutes,2) + ":" + this.pad(seconds,2);
          }
      }
  }
</script>
<style scoped>
  
  p {
    font-size: 2em;
  }

</style>  