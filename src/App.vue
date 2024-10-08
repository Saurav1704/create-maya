<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay">
        <ion-content>
          <br />
          <ion-list-header router-link="/Main/MAYA" @click="closeSplitPane">
            <ion-img :src="mayaLogo" class="mayalogosvg" alt="Maya" style="max-width:35px; margin-top:3px; " />
            &nbsp;
            <h2>MAYA</h2>
          </ion-list-header>
          <br />
          <ion-list id="inbox-list">
            <ion-menu-toggle :auto-hide="false" v-for="(p, i) in appPages" :key="i">
              <ion-item @click="handleMenuClick(i)" router-direction="root" :router-link="p.url" lines="none"
                :detail="false" class="hydrated" :class="{ selected: selectedIndex === i }">
                <ion-icon aria-hidden="true" slot="start" :ios="p.iosIcon" :md="p.mdIcon"></ion-icon>
                <ion-label>{{ p.title }}</ion-label>
              </ion-item>
            </ion-menu-toggle>
          </ion-list>
          <ion-img :src="ssgLogo" alt="Sopra Steria" class="menu-logo"></ion-img>
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script>
import {
  IonApp,
  IonContent,
  IonIcon,
  IonItem,
  IonLabel,
  IonImg,
  IonList,
  IonListHeader,
  IonMenu,
  IonMenuToggle,
  IonRouterOutlet,
  IonSplitPane,
} from '@ionic/vue';
import { menuController } from '@ionic/vue';
import { StatusBar } from '@capacitor/status-bar';
import { Capacitor } from '@capacitor/core';
import SsgLogo from '@/assets/sopra-steria.png';
import MayaLogo from '@/assets/maya-logo.svg';
import BackgroundImage from '@/assets/background-image.jpg';
import { App } from '@capacitor/app';
import { chatbubbles, cloudy, codeWorkingOutline,documentSharp, documentTextOutline, documentOutline } from 'ionicons/icons';

export default {
  components: {
    IonApp,
    IonContent,
    IonIcon,
    IonItem,
    IonLabel,
    IonImg,
    IonList,
    IonListHeader,
    IonMenu,
    IonMenuToggle,
    IonRouterOutlet,
    IonSplitPane,
  },
  data() {
    return {
      selectedIndex: -1,
      ssgLogo: SsgLogo,
      mayaLogo: MayaLogo,
      statusBar: StatusBar,
      appPages: [
        {
          title: 'Bill of Material',
          url: '/Main/Bill of Material',
          iosIcon: documentTextOutline,
          mdIcon: documentTextOutline,
          filename: 'prompt_bom.txt',
        },
        {
          title: 'Purchase Order',
          url: '/Main/Purchase Order',
          iosIcon: documentOutline,
          mdIcon: documentOutline,
          filename: 'prompt_po.txt',
        },
        {
          title: 'Material Master',
          url: '/Main/General Query',
          iosIcon: documentSharp,
          mdIcon: documentSharp,
          filename: 'prompt_query.txt',
        },
      ],
    };
  },
  created() {
    if (Capacitor.isNativePlatform()) {
      // The code will only run on native platforms (iOS or Android)
      try {
        StatusBar.setBackgroundColor({ color: '#5c9eda' });
        StatusBar.setOverlaysWebView({ overlay: false });
      } catch (e) {
        console.error('Error setting the status bar:', e);
      }
    } else {
      console.log("StatusBar plugin is not available on web.");
    }

    App.addListener('backButton', () => {
      if (window.location.pathname === '/Main/MAYA') App.exitApp();
      history.back();
    });

    const path = window.location.pathname.split('Main/')[1];
    if (path !== undefined) {
      this.selectedIndex = this.appPages.findIndex((page) => page.title.toLowerCase() === path.toLowerCase());
    }
  },
  methods: {
    async closeSplitPane() {
      await menuController.close();
      this.handleMenuClick(-1);
    },
    handleMenuClick(index) {
      if (index === -1) {
        this.selectedIndex = '';
      } else {
        this.selectedIndex = index;
        const selectedPage = this.appPages[index];
        sessionStorage.setItem('filename', selectedPage.filename);
        sessionStorage.setItem('title', selectedPage.title);
      }
    },
  },
};
</script>

<style scoped>
ion-menu ion-content {
  --background: none;
  background-image: url('@/assets/background-image.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  --padding-bottom: 20px;
}

ion-menu.md ion-content {
  --padding-start: 8px;
  --padding-end: 8px;
  --padding-top: 20px;
  --padding-bottom: 20px;
}

ion-menu.md ion-list {
  padding: 20px 0;
}

ion-menu.md ion-note {
  margin-bottom: 30px;
}

ion-menu.md ion-list-header,
ion-menu.md ion-note {
  padding-left: 10px;
}

ion-menu.md ion-list#inbox-list {
  border-bottom: 1px solid var(--ion-background-color-step-150, #d7d8da);
}

ion-menu.md ion-list#inbox-list ion-list-header {
  font-size: 22px;
  font-weight: 600;
  margin-left: 50px;
  min-height: 20px;
}

ion-menu.md ion-list#labels-list ion-list-header {
  font-size: 16px;
  margin-bottom: 18px;
  color: #757575;
  min-height: 26px;
}

ion-menu.md ion-item {
  --padding-start: 10px;
  --padding-end: 10px;
  border-radius: 4px;
}

ion-menu.md ion-item.selected {
  --background: rgba(var(--ion-color-primary-rgb), 0.14);
}

ion-menu.md ion-item.selected ion-icon {
  color: var(--ion-color-primary);
}

ion-menu.md ion-item ion-icon {
  color: #616e7e;
}

ion-menu.md ion-item ion-label {
  font-weight: 500;
}

ion-menu.ios ion-content {
  --padding-bottom: 20px;
}

ion-menu.ios ion-list {
  padding: 20px 0 0 0;
}

ion-menu.ios ion-note {
  line-height: 24px;
  margin-bottom: 20px;
}

ion-menu.ios ion-item {
  --padding-start: 16px;
  --padding-end: 16px;
  --min-height: 50px;
  margin-left: 50px;
}

ion-menu.ios ion-item.selected ion-icon {
  color: var(--ion-color-primary);
}

ion-menu.ios ion-item ion-icon {
  font-size: 24px;
  color: #73849a;
}

ion-menu.ios ion-list#labels-list ion-list-header {
  margin-bottom: 8px;
}

ion-menu.ios ion-list-header,
ion-menu.ios ion-note {
  padding-left: 16px;
  padding-right: 16px;
}

ion-menu.ios ion-note {
  margin-bottom: 8px;
}

ion-note {
  display: inline-block;
  font-size: 16px;
  color: var(--ion-color-medium-shade);
}

ion-item.selected {
  --color: var(--ion-color-primary);
}

ion-img.menu-logo {
  position: absolute;
  bottom: 10px;
  left: 10px;
  width: 100px;
  height: auto;
}

ion-img.menu-maya-logo {
  position: absolute;
  top: 10px;
  left: 10px;
  width: 40px;
  height: 60px;
}
ion-img::part(image) {
  border-radius: 40%;
}
</style>