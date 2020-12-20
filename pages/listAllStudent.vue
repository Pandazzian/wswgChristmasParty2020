<template>
  <div style="background-color:rgb(0,51,89)">
    <div v-for="index in Array(50).keys()" :key="`snow-${index}`" class="snow" />
    <div class="row">
      <div class="pt-2 col-5 ml-auto mr-auto" style="text-align:center;">
          <img class="logo-container" style="max-width:100%" src="~assets/download.png" @click="toHome()">
      </div>
    </div>
    <h1 class="text-center pt-5 pb-5" style="color:white">
      WSE Westgate<br/>Christmas Party 2020
    </h1>
    
    <h2 class="text-center" style="color:white">Total Participant: {{totalStudent}}</h2>
    <div class="row">
        <b-form-group class="col-lg-6 col-12 pl-2 pr-2" style="color:white" id="input-group-2" label="search Number:" label-for="id">
        <b-form-input
            min="0"
            id="id"
            type="number"
            v-model="filterid"
            placeholder="Enter number"
        ></b-form-input>
        </b-form-group>
    </div>
    <div class="row">
        <div class="col-lg-2 col-12 pl-2 pr-2">
            <button @click="filter()" type="button" class="mt-2 mb-2 w-100 btn btn-primary">search</button>
        </div>
        <div class="col-lg-2 col-12 pl-2 pr-2">
            <button @click="reset()" type="button" class="mt-2 mb-2 w-100 btn btn-secondary">reset</button>
        </div>
    </div>
    <!-- <div class="pl-2 pr-2">
        <button @click="sortParticipant()" type="button" class="mt-2 mb-2 w-100 btn btn-info">sort</button>
    </div> -->
    <studentTable :filteredParticipant="filteredParticipant"/>
    <div class="pt-5 pb-5"/>
  </div>
</template>

<script>
import { db } from '~/plugins/firebase.js'
import studentTable from '~/components/table'

export default {
  data(){
    return{
      totalStudent:null,
      allParticipant:[],
      filteredParticipant:[],
      filterid:''
    }
  },
  components:{
      studentTable,
  },
  mounted(){
      this.getParticipant();
      this.getAllParticipant();
  },
  methods:{
    sortParticipant(){
        this.filteredParticipant.sort((itema,itemb)=>{
            let a = parseInt(itema.id)
            let b = parseInt(itemb.id)
            if(a<b){
                return -1
            }else if(a>b){
                return 1
            }else{
                return 0
            }
        })
    },
    
    filter(){
        // console.log('pien',this.allParticipant,'filter',this.filterid=='');
        this.filteredParticipant = this.allParticipant.filter(each=>
            (this.filterid == '')||(each.id == this.filterid)
        )
        this.sortParticipant()
    },
    reset(){
        this.filterid = '';
        this.filter();
        this.sortParticipant();
    },
    async getParticipant(){
        // var Participant = null
        db.collection("runningNumber").doc("number")
        .onSnapshot((doc) => {
            console.log("Current data: ", doc.data());
            this.totalStudent = doc.data().latest
        });
    },
    async getAllParticipant(){
        db.collection("reg-users").get().then((querySnapshot) => {
            querySnapshot.forEach((doc) => {
                // doc.data() is never undefined for query doc snapshots
                // console.log(doc.id, " => ", doc.data());
                let participant ={ 
                    id: doc.id,
                    data: doc.data(),
                    // name: doc.data().name,
                    // surname: doc.data().surname,
                    // studentCode: doc.data().studentCode,
                    // phoneNumber: doc.data().phoneNumber
                    }
                this.allParticipant.push(participant)
                this.filter()
                // console.log(this.getParticipant)
            });
        });
    }
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
.row{
    margin:0;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.logo-padding{
  padding-right: 8rem;
  padding-left: 8rem;
}
.logo-container{
  border: 5px solid white;
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
