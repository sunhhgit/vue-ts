<template>
  <div class="home">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>

    <img alt="Vue logo" src="../assets/logo.png">

    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>

    <div style="margin: 20px auto">倒计时：{{timeDisplay}}</div>

    <button class="q-mt-lg" @click="toStopCountDown">点击停止倒计时</button>
    <button class="q-mt-lg" style="margin-left: 16px" @click="toContinueCountDown">点击继续倒计时</button>
    <br/>
    <button class="q-mt-lg" @click="toAboutPage">点击跳转到About页面(*必传参数testName)</button>
    <br/>
    <button class="q-mt-lg" @click="toUserPage">点击跳转到User页面(*必传参数userId)</button>
  </div>
</template>

<script lang="ts">
  import {Component, Vue} from 'vue-property-decorator';
  import HelloWorld from '../components/HelloWorld.vue';
  import {RouterHelper} from '../lib/routerHelper';
  import {Countdown, CountdownEventName, fillZero} from '../lib/countdown';
  import {RoutePath} from '../router';
  import {measure} from '../decorator';

  @Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
    public timeDisplay: string = '';
    public countdownObj: any = null;

    @measure
    public toAboutPage() {
        RouterHelper.push(RoutePath.About, {  testName: '1111' });
    }

    @measure
    public toUserPage() {
        RouterHelper.push(RoutePath.User, { userId: '123' })
    }

    public created() {
        const countdown = new Countdown(Date.now() + 60 * 60 * 1000, 10);
        this.countdownObj = countdown
        countdown.on(CountdownEventName.RUNNING, (remainTimeData) => {
            const { hours, minutes, seconds, count} = remainTimeData;
            this.timeDisplay = [hours, minutes, seconds, count].map(fillZero).join(':');
        });

    }

    public toStopCountDown() {
      this.countdownObj.stop()
    }

    public toContinueCountDown() {
      this.countdownObj.start()
    }
}
</script>

<style lang="less">
  #nav {
    padding: 30px;

    a {
      font-weight: bold;
      color: #2c3e50;

      &.router-link-exact-active {
        color: #42b983;
      }
    }
  }
  .q-mt-lg{
    margin-top: 32px;
  }
</style>
