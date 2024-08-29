<script setup lang="ts">
import {createPlayerClient, PlayerClient} from "@reveldigital/client-sdk";
import {reactive} from "vue";


  const title = 'vue-test-client-sdk';
  let sdk: PlayerClient;
  let revel = reactive({
    state : 'Not ready',
    localTime: "",
    deviceTime: '',
    TZName: '',
    TZId: '',
    TZOffset: '',
    langCode: '',
    deviceKey: '',
    revelRoot: '',
    remoteDeviceKey: '',
    commandMap: '',
    prefs: '',
    style: ''
  })

  revel.state = 'ready'// Initialize the SDK

  sdk = createPlayerClient({
    useLegacyEventHandling: true
  });
  //
  // sdk.on(EventType.START,  ()=> {
  //   state = 'started'
  //   console.log('Player started');
  //   sdk.getDevice().then(function (device) {
  //     console.log('Device: ' + device);
  //   })
  // });
  //
  // sdk.on(EventType.COMMAND, function (data) {
  //   console.log('Command received: ' + data);
  // });

  sdk.isPreviewMode().then(function (isPreview) {
    console.log('Is preview mode: ' + isPreview);
  });


  setInterval(() => {
    update();
  }, 1000);

  function update() {
    sdk.getDeviceTime().then((res) => {
      revel.localTime = (new Date()).toString();
      // @ts-ignore
      //todo fix
      revel.deviceTime = new Date(res);
    });

    sdk.getDeviceTimeZoneName().then((res) => {
      revel.TZName = res? res : '';
    });

    sdk.getDeviceTimeZoneID().then((res) => {
      revel.TZId = res? res : '';
    })

    sdk.getDeviceTimeZoneOffset().then((res) => {
      revel.TZOffset = res? res.toString() : '';
    });

    sdk.getLanguageCode().then((res) => {
      revel.langCode = res? res : '';
    });

    sdk.getDeviceKey().then((res) => {
      revel.deviceKey = res? res : '';
    });

    sdk.getRevelRoot().then((res) => {
      revel.revelRoot = res? res : '';
    });

    sdk.getCommandMap().then((res) => {
      revel.commandMap = res? res : '';
    });
  }

  function sendCommand() {
    sdk.sendCommand("test", "it");
  }



  function trackEvent() {
    sdk.track("test", { "a": "b" });
  }

  function callback() {
    sdk.callback('test');
  }
</script>

<template>
  <h2>{{title}}</h2>
  <h2>State: {{ revel.state }}</h2>
  <h2>Local Time: {{ revel.localTime }}</h2>
  <h2>Device Time: {{ revel.deviceTime }}</h2>
  <h2>Device TZ Name: {{ revel.TZName }}</h2>
  <h2>Device TZ ID: {{ revel.TZId }}</h2>
  <h2>Device TZ Offset: {{ revel.TZOffset }}</h2>
  <h2>Device Language Code: {{ revel.langCode }}</h2>
  <h2>Device Key: {{ revel.deviceKey }}</h2>
  <h2>Revel Root: {{ revel.revelRoot }}</h2>
  <h2>Command Map: {{ revel.commandMap }}</h2>
  <button @click="sendCommand()">Send Command</button>
  <button @click="trackEvent()">Track Event</button>
  <button @click="callback()">Callback</button>
</template>
