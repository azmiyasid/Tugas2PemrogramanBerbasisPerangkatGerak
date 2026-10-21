<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Prakiraan Cuaca Jakarta</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content class="ion-padding">
      <ion-list>
        <ion-item v-for="(item, index) in weatherList" :key="index">
          <ion-label>
            <h2>Waktu: {{ item.time }}</h2>
            <p>Suhu: {{ item.temperature }} °C</p>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonList, IonItem, IonLabel } from '@ionic/vue';

const weatherList = ref([]);

onMounted(async () => {
  try {
    const response = await fetch('https://api.open-meteo.com/v1/forecast?latitude=-6.2&longitude=106.8&hourly=temperature_2m');
    const data = await response.json();

    const times = data.hourly.time.slice(0, 10);
    const temperatures = data.hourly.temperature_2m.slice(0, 10);

    weatherList.value = times.map((timeString, index) => {
      return {
        time: timeString.replace('T', ' '),
        temperature: temperatures[index]
      };
    });

  } catch (error) {
    console.error('Gagal mengambil data cuaca:', error);
  }
});
</script>