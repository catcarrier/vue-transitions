<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <div><button class="btn btn-primary" @click="show = !show">Show alert</button></div>
                <br>
                <select name="" id="" v-model="alertAnimation" class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <br>

                <!-- <transition name="fade">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>
                <transition name="slide">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>
                <div>other stuff</div>

                <transition 
                    enter-active-class="animated bounce"
                    leave-active-class="animated shake"
                    >
                    <div class="alert alert-info" v-if="show">This is some other info</div>
                </transition> -->

                 <!-- <transition :name="alertAnimation" mode="out-in">
                    <div class="alert alert-info" v-if="show" key="info">This is some info</div>
                    <div class="alert alert-warning" v-else key="warning">This is some warning</div>
                </transition> -->

                <hr>
                <h2>JS Animation</h2>
                <button class="btn btn-primary" @click="toggleLoad">Load/Remove element</button>
                <br><br>
                <transition
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @after-enter="afterEnter"
                    @enter-cancelled="enterCancelled"
                    @before-leave="beforeLeave"
                    @leave="leave"
                    @after-leave="afterLeave"
                    @leave-cancelled="leaveCancelled"
                    :css="false">
                    <div style="width:300px; height:100px; background-color: lightgreen;" v-if="load"></div>
                </transition>

                <hr>
                <button class="btn btn-primary" @click="selectedComponent == 'app-alert-success' ? selectedComponent = 'app-alert-danger' : selectedComponent = 'app-alert-success'">toggle components</button>
                <br><br>
                <transition name="fade" mode="out-in">
                    <component :is="selectedComponent"></component>
                </transition>

                



                
            </div>
        </div>
    </div>
</template>

<script>
import alertDanger from './DangerAlert.vue';
import alertSuccess from './SuccessAlert.vue';

export default {
  data() {
    return {
      show: false,
      alertAnimation: "fade",
      load: true,
      elementWidth: 100,
      selectedComponent: 'app-alert-success'
    };
  },
  methods: {
    toggleLoad() {
      this.load = !this.load;
    },
    beforeEnter: function(el) { 
        console.log('beforeEnter');
        this.elementWidth=100;
        el.style.width = this.elementWidth + 'px';
    },
    enter: function(el, done) { 
        console.log('enter'); 

        /**
         * Animate the width up to (20 *10px) + 100px which is original width, or 300px
         * 
         */
        let round = 1;
        const interval = setInterval( ()=> {
            el.style.width = (this.elementWidth + (round * 10)) + 'px';
            round++;
            if(round>20) { 
                clearInterval(interval);
                done();
            }
        }, 20 );
    },
    afterEnter: function(el) { console.log('afterEnter'); },
    enterCancelled: function(el) { console.log('enterCancelled'); },
    beforeLeave: function(el) { 
        console.log('beforeLeave'); 
        this.elementWidth=300;
        el.style.width = this.elementWidth + 'px';
    },
    leave: function(el, done) { 
        console.log('leave'); 
        let round = 1;
        const interval = setInterval( ()=> {
            el.style.width = (this.elementWidth - (round * 10)) + 'px';
            round++;
            if(round>20) { 
                clearInterval(interval);
                done();
            }
        }, 20 ); 
    },
    afterLeave: function(el) { console.log('afterLeave'); },
    leaveCancelled: function(el) { console.log('leaveCancelled'); }
  },
  components: {
      'app-alert-success': alertSuccess,
      'app-alert-danger': alertDanger
  }
};
</script>

<style>
.fade-enter {
  opacity: 0;
}

.fade-enter-active {
  transition: opacity 0.5s;
}

.fade-leave {
}

.fade-leave-active {
  transition: opacity 0.5s;
  opacity: 0;
}

.slide-enter {
  /* transform: translateY(20px); */
  opacity: 0;
}

.slide-enter-active {
  animation: slide-in 1s ease-out forwards; /* keep elem at final position, no snap-back */
  transition: opacity 0.5s;
}

.slide-leave {
}

.slide-leave-active {
  animation: slide-out 1s ease-out forwards;
  transition: opacity 1s;
  opacity: 0;
}

@keyframes slide-in {
  from {
    transform: translateY(20px);
  }
  to {
    transform: translateY(0px);
  }
}

@keyframes slide-out {
  from {
    transform: translateY(0px);
  }
  to {
    transform: translateY(20px);
  }
}
</style>
