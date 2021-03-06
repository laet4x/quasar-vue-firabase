<template>
 <q-layout>
  <!-- Header -->
  <div slot="header" class="toolbar">
    <!-- opens left-side drawer using its ref -->
    <button class="hide-on-drawer-visible" @click="$refs.leftDrawer.open()">
      <i>menu</i>
    </button>
    <q-toolbar-title :padding="1">
      Home
    </q-toolbar-title>
    <!-- opens right-side drawer using its ref -->
    <!-- <button class="hide-on-drawer-visible" @click="$refs.rightDrawer.open()">
        <i>menu</i>
    </button> -->
  </div>
  <!-- Navigation Tabs -->
  <q-tabs slot="navigation">
    <q-tab icon="home" route="/" exact replace>Home</q-tab>
    <q-tab icon="info" route="/about" exact replace>Info</q-tab>
    <q-tab icon="add" route="/books" exact replace>Add Books</q-tab>
  </q-tabs>
  <!-- Left-side Drawer -->
  <q-drawer ref="leftDrawer">
    <div class="toolbar">
      <q-toolbar-title>
        Menu
      </q-toolbar-title>
    </div>
    <div class="list no-border platform-delimiter">
      <q-drawer-link icon="mail" :to="{path: '/', exact: true}">
        Link
      </q-drawer-link>
    </div>
  </q-drawer>
  <!-- IF USING subRoutes only: -->
  <router-view class="layout-view"></router-view>
  <!-- OR ELSE, IF NOT USING subRoutes: -->
 <!--  <div class="layout-view"></div> -->
  <!-- Right-side Drawer -->
 <!--  <q-drawer ref="rightDrawer" right-side>
    ...
  </q-drawer> -->
  <!-- Footer -->
  <div slot="footer" class="toolbar">
    @company name
  </div>
 </q-layout>
</template>

<script>
const moveForce = 30
const rotateForce = 40
const RAD_TO_DEG = 180 / Math.PI

import { Utils, Platform } from 'quasar'

function getRotationFromAccel (accelX, accelY, accelZ) {
  /* Reference: http://stackoverflow.com/questions/3755059/3d-accelerometer-calculate-the-orientation#answer-30195572 */
  const sign = accelZ > 0 ? 1 : -1
  const miu = 0.001

  return {
    roll: Math.atan2(accelY, sign * Math.sqrt(Math.pow(accelZ, 2) + miu * Math.pow(accelX, 2))) * RAD_TO_DEG,
    pitch: -Math.atan2(accelX, Math.sqrt(Math.pow(accelY, 2) + Math.pow(accelZ, 2))) * RAD_TO_DEG
  }
}

export default {
  data () {
    return {
      orienting: window.DeviceOrientationEvent && !Platform.is.desktop,
      rotating: window.DeviceMotionEvent && !Platform.is.desktop,
      moveX: 0,
      moveY: 0,
      rotateY: 0,
      rotateX: 0
    }
  },
  computed: {
    position () {
      const transform = `rotateX(${this.rotateX}deg) rotateY(${this.rotateY}deg)`
      return {
        top: this.moveY + 'px',
        left: this.moveX + 'px',
        '-webkit-transform': transform,
        '-ms-transform': transform,
        transform
      }
    }
  },
  methods: {
    move (evt) {
      const {width, height} = Utils.dom.viewport()
      const {top, left} = Utils.event.position(evt)
      const halfH = height / 2
      const halfW = width / 2

      this.moveX = (left - halfW) / halfW * -moveForce
      this.moveY = (top - halfH) / halfH * -moveForce
      this.rotateY = (left / width * rotateForce * 2) - rotateForce
      this.rotateX = -((top / height * rotateForce * 2) - rotateForce)
    },
    rotate (evt) {
      if (evt.rotationRate &&
          evt.rotationRate.beta !== null &&
          evt.rotationRate.gamma !== null) {
        this.rotateX = evt.rotationRate.beta * 0.7
        this.rotateY = evt.rotationRate.gamma * -0.7
      }
      else {
        /* evt.acceleration may be null in some cases, so we'll fall back
           to evt.accelerationIncludingGravity */
        const accelX = evt.acceleration.x || evt.accelerationIncludingGravity.x
        const accelY = evt.acceleration.y || evt.accelerationIncludingGravity.y
        const accelZ = evt.acceleration.z || evt.accelerationIncludingGravity.z - 9.81

        const rotation = getRotationFromAccel(accelX, accelY, accelZ)
        this.rotateX = rotation.roll * 0.7
        this.rotateY = rotation.pitch * -0.7
      }
    },
    orient (evt) {
      if (evt.beta === null || evt.gamma === null) {
        window.removeEventListener('deviceorientation', this.orient, false)
        this.orienting = false

        window.addEventListener('devicemotion', this.rotate, false)
      }
      else {
        this.rotateX = evt.beta * 0.7
        this.rotateY = evt.gamma * -0.7
      }
    }
  },
  mounted () {
    this.$nextTick(() => {
      if (this.orienting) {
        window.addEventListener('deviceorientation', this.orient, false)
      }
      else if (this.rotating) {
        window.addEventListener('devicemove', this.rotate, false)
      }
      else {
        document.addEventListener('mousemove', this.move)
      }
    })
  },
  beforeDestroy () {
    if (this.orienting) {
      window.removeEventListener('deviceorientation', this.orient, false)
    }
    else if (this.rotating) {
      window.removeEventListener('devicemove', this.rotate, false)
    }
    else {
      document.removeEventListener('mousemove', this.move)
    }
  }
}
</script>

<style lang="stylus">
.logo-container
  width 192px
  height 268px
  perspective 800px
  position absolute
  top 50%
  left 50%
  transform translateX(-50%) translateY(-50%)
.logo
  position absolute
  transform-style preserve-3d
</style>
