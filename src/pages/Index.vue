<template>
<div>
  <div style=" height: 250px; margin-left: 1520px; back">
  <div style="font-size: 25px; font-weight: bold; margin-bottom: 7px; ">Logg</div>
        <q-scroll-area style="height: 250px; width: 300px; border:groove green; border-radius: 10px; background-color:white;">

            <div v-for="(logg, index) in loggar" :key="index" class="caption">
                <p style="padding-left: 5px; margin-left: 5px">{{ logg.time }}   {{ logg.value }} </p>
            </div>
        </q-scroll-area>

    <!-- <q-btn label="Logg info" style="text-color: black;" color="black" @click="alert = true" />
    <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Logg information</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Datum : MQTT topic : Värde i fart/grader

         (positivt värde är framåt(speed)/höger(angle) medans negativt är bakåt/vänster)
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog> -->
    </div>
  <div id="index">
    <JoyStick @change="handleChange('left', $event);" />
  </div>

  </div>
</template>

<script>
import { client } from "../boot/mqtt-boot";
import JoyStick from "../components/JoyStick";
import { ref } from 'vue';
export default {
  name: "index",
  components: {
    JoyStick
  },
  created(){
    client.on("message", (topic, message) => {
      let today = new Date();
      let fullDate =
        today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
      this.loggar.unshift({ time: fullDate, value: topic + " " + message });
      //this.loggar.unshift(`${topic} - ${message.toString()}`)
    });
    client.on("connect", () => {
      console.log(`Connected`);
      client.subscribe("chrjer/#",function(err){});
    });
  },
  data() {
    return {
      leftStick: {
        y: 0,
        speed: 0,
        angle: 0,
      },
      loggar:[],
      alert: ref(false),
    };
  },
  methods: {
    handleChange(id, { y, speed, angle }) {
      const stick = this[`${id}Stick`];
      stick.y = y;
      stick.speed = speed;
      stick.angle = angle;
      if(y<0){
        client.publish("chrjer/speed", String(speed));
        }
      else{
        client.publish("chrjer/speed", String(-1*speed));
        }
        client.publish("chrjer/angle", String(angle));
    },
  },
};
</script>

<style>
#index {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 50px;
  margin-left: 10px;
  /* background-color:darkgrey */
}
</style>

