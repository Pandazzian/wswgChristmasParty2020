<template>
  <div style="background-color:rgb(0,51,89);">
    <div v-for="index in Array(50).keys()" :key="`snow-${index}`" class="snow" />
    <div class="row">
      <div class="pt-2 col-5 ml-auto mr-auto" style="text-align:center;">
          <img class="logo-container" style="max-width:100%" src="~assets/download.png" @click="toHome()">
      </div>
    </div>
    <h1 class="text-center pt-5 pb-5" style="color:white">
      WSE Westgate<br/>Christmas Party 2020
    </h1>
    <!-- name surname student code phone number -->
    <b-form-group class="pl-2 pr-2" style="color:white" id="input-group-2" label="Name:" label-for="input-2">
      <b-form-input
        id="name"
        v-model="name"
        placeholder="Enter name"
        required
      ></b-form-input>
    </b-form-group>
    <b-form-group class="pl-2 pr-2" style="color:white" id="input-group-2" label="Surname:" label-for="input-2">
      <b-form-input
        id="surname"
        v-model="surname"
        placeholder="Enter surname"
        required
      ></b-form-input>
    </b-form-group>
    <b-form-group class="pl-2 pr-2" style="color:white" id="input-group-2" label="Student Code (optional):" label-for="input-2">
      <b-form-input
        id="studentCode"
        v-model="studentCode"
        placeholder="Enter student code"
      ></b-form-input>
    </b-form-group>
    <b-form-group class="pl-2 pr-2" style="color:white" id="input-group-2" label="Contact Number:" label-for="input-2">
      <b-form-input
        id="phone"
        type="number"
        v-model="phoneNumber"
        placeholder="Enter your phone number"
        required
      ></b-form-input>
    </b-form-group>
    <div class="pl-2 pr-2 pt-5">
      <b-button @click="submit()" class="w-100" variant="success">Register</b-button>
    </div>
      <!-- <h3 class="text-center pt-3 pb-3" style="color:white">
        or
      </h3>
    <div class="pl-2 pr-2">
      <b-button v-b-modal.modal-1 class="w-100" variant="primary">Already Registered</b-button>
    </div>
    <b-modal @ok="already()" id="modal-1" title="Already Registered" ok-title="submit">
      <b-form-group class="pl-2 pr-2" id="input-group-2" label="Name:" label-for="input-2">
        <b-form-input
          id="name"
          v-model="regName"
          placeholder="Enter name"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group class="pl-2 pr-2" id="input-group-2" label="Registered Phone Number:" label-for="input-2">
        <b-form-input
          id="phone"
          type="number"
          v-model="regPhone"
          placeholder="Enter registered phone number"
          required
        ></b-form-input>
      </b-form-group>
    </b-modal> -->
    <div class="text-right pl-2 pr-2 pt-4" style="color:white;font-size:0.75rem">
      Powered by Pongsawut Naknual tel.0849949261
    </div>
  </div>
</template>

<script>
import { db } from '~/plugins/firebase.js'

export default {
  data(){
    return{
      name:'',
      surname:'',
      studentCode: null,
      phoneNumber:'',
      id:null,
      regPhone : '',
      regName:'',
      userswithPhoneNumber: [],
    }
  },
  mounted(){
  },
  methods:{
    async already(){
      db.collection("reg-users").where("phoneNumber", "==", this.regPhone)
      .get()
      .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
              // doc.data() is never undefined for query doc snapshots
              console.log(doc.id, " => ", doc.data());
              this.userswithPhoneNumber.push({id:doc.id,name:doc.data().name})
          });
          let id = this.userswithPhoneNumber.filter(each=>each.name == this.regName)[0].id
          this.$router.push({ path: `/registered?id=${id}` })
      })
      .catch(function(error) {
          console.log("Error getting documents: ", error);
      });
    },
    async submit(){
      console.log(process.env.VUE_APP_MEASUREMENTID);
      
      var sfDocRef = db.collection("runningNumber").doc("number");
      var myname = this.name;
      var mysurname = this.surname;
      var mystudentCode = this.studentCode;
      var myPhoneNumber = this.phoneNumber;
      const id = await db.runTransaction(function(transaction) {
          // This code may get re-run multiple times if there are conflicts.
          return transaction.get(sfDocRef).then(function(sfDoc) {
              if (!sfDoc.exists) {
                  throw "Document does not exist!";
              }
              // db.db.collection("runningNumber").doc("number")
              // Add one person to the city population.
              // Note: this could be done without a transaction
              //       by updating the population using FieldValue.increment()
              var newlatest = sfDoc.data().latest + 1;
              console.log('newlatest',newlatest);
              transaction.update(sfDocRef, { latest: newlatest });
              transaction.set(db.collection('reg-users').doc(newlatest.toString()),{
                name:myname,
                surname:mysurname,
                phoneNumber:myPhoneNumber,
                studentCode:mystudentCode,
              })
              return newlatest;
          });
      }).then(function(id) {
          console.log("Transaction successfully committed!");
          return id
      }).catch(function(error) {
          console.log("Transaction failed: ", error);
      });
      if(id != null){
        this.$router.push({ path: `/registered?id=${id}` })
      }
    },
  }
}
</script>

<style lang="scss" scoped>
body {
  height: 100vh;
  background: radial-gradient(ellipse at bottom, #1b2735 0%, #090a0f 100%);
  overflow: hidden;
  filter: drop-shadow(0 0 10px white);
}

@function random_range($min, $max) {
  $rand: random();
  $random_range: $min + floor($rand * (($max - $min) + 1));
  @return $random_range;
}

.snow {
  $total: 200;
  position: absolute;
  width: 10px;
  height: 10px;
  background: white;
  border-radius: 50%;

  @for $i from 1 through $total {
    $random-x: random(1000000) * 0.0001vw;
    $random-offset: random_range(-100000, 100000) * 0.0001vw;
    $random-x-end: $random-x + $random-offset;
    $random-x-end-yoyo: $random-x + ($random-offset / 2);
    $random-yoyo-time: random_range(30000, 80000) / 100000;
    $random-yoyo-y: $random-yoyo-time * 100vh;
    $random-scale: random(10000) * 0.0001;
    $fall-duration: random_range(10, 30) * 1s;
    $fall-delay: random(30) * -1s;

    &:nth-child(#{$i}) {
      opacity: random(10000) * 0.0001;
      transform: translate($random-x, -10px) scale($random-scale);
      animation: fall-#{$i} $fall-duration $fall-delay linear infinite;
    }

    @keyframes fall-#{$i} {
      #{percentage($random-yoyo-time)} {
        transform: translate($random-x-end, $random-yoyo-y) scale($random-scale);
      }
      
      to {
        transform: translate($random-x-end-yoyo, 100vh) scale($random-scale);
      }
    }
  }
}
.logo-padding{
  padding-right: 8rem;
  padding-left: 8rem;
}
.logo-container{
  border: 5px solid white;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
