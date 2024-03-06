<script setup>

import { initializeApp } from "firebase/app";
import { ref } from 'vue'
import { getMessaging, getToken, onMessage } from "firebase/messaging";
const firebaseConfig = {
  apiKey: "AIzaSyAjJdt0cqts6XRjrQ7B6_Wea_LOLYNnriI",
  authDomain: "send-noti-tutorial.firebaseapp.com",
  projectId: "send-noti-tutorial",
  storageBucket: "send-noti-tutorial.appspot.com",
  messagingSenderId: "580978714195",
  appId: "1:580978714195:web:b8b1b25b532dce22c608a6",
  measurementId: "G-M8C2GYSVQR"
};
const token = ref("")
const errMsg = ref("")
const permission = ref("")
const app = initializeApp(firebaseConfig);
const messaging = getMessaging(app);


onMessage(messaging, (payload) => {
  console.log('Message received. ', payload);
});
getToken(messaging, { vapidKey: 'BMqZQbTGzSVLlNHZRpP84G0d2A5mGg-4XazH3SuoAWIWfL54E7jIG_EX6QG74KjxlfmmR2k-UNizREEGG6bdB0k' }).then((currentToken) => {
  if (currentToken) {
    console.log("currentToken from messaging: ", currentToken)
    token.value = currentToken
  } else {
    // Show permission request UI
    console.log('No registration token available. Request permission to generate one.');
    // ...
  }
}).catch((err) => {
  console.log('An error occurred while retrieving token. ', err);
  // ...
});


function notifyMe() {
  if (!("Notification" in window)) {
    errMsg.value = "This browser does not support desktop notification"
  } else if (Notification.permission === "granted") {
    // Permission has already been granted
    console.log("Notification permission already granted");
  } else if (Notification.permission !== "denied") {
    // Request permission for notifications
    Notification.requestPermission().then(function (result) {
      permission.value = result
      if (result === "granted") {
        // Permission granted, you can now create notifications
        console.log("Notification permission granted");
      } else if (result === "denied") {
        // Permission denied
        console.log("Notification permission denied");
      } else if (result === "default") {
        // The user closed the permission prompt without making a choice
        console.log("User closed the permission prompt without making a choice");
      }
    });
  }

}






</script>

<template>
  <div>
    test
    tokennn
    {{ token }}
    <button @click="notifyMe">Notify me!</button>
    {{ errMsg }}
    {{ permission }}
  </div>
</template>


<style scoped></style>
