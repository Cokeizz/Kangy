<template>
  <div class="d-flex justify-content-center">
    <!-- Section1 : Input Number of Player -->
    <div v-if="currentState == '0'" class="container">
      <div>
        <h1 class="">How many players?</h1><br>
        <CButton @click="setNumberPlayer(i+1)" v-for="i in 5" style="" color="light"  
        :class="{ 'm-1': true,' text-dark':true ,'button-number': true, 'check-control': activeButton === i+1 }">{{ i+1 }}</CButton>
      </div>
      <div class="mt-5">
        <h1 class="">Charge/Round?</h1><br>
        <CForm>
  <CFormInput
  v-model="charge"
    type="number"
    placeholder=""
    text="Must be more than 1$ per round"
  />
</CForm>
      </div>
      <div class="mt-5">
        <h1 class="">Ace charge/Card?</h1><br>
        <CForm>
  <CFormInput
  v-model="ace"
    type="number"
    placeholder=""
    text="Must be more than 0$ per round"
  />
</CForm>
      </div>
      <CButton style="width: 100%;" @click="saveState0()" class="mt-4" color="primary">Confirm</CButton>
    </div>

     <!-- Section2 : Input Player Details -->
     <div v-if="currentState == '1'" class="container ">
      <div class="">
        <h1 class="m-2 mb-4">What is your name?</h1>
        <div class="row">
          <div v-for="i in parseInt(numberOfPlayer)" class="input-box m-1">
        <h2>P{{ i }}</h2><br>
        <CFormInput
    type="text"
    id="username"
    placeholder="Name"
    v-model="playername[i-1]"
  />
          </div>
          <CButton @click="savePlayer()" class="mt-4" color="primary">Confirm</CButton>
        </div>
      </div>
    </div>

    <!-- Section3 : Game Play -->
    <div v-if="currentState == '2'" class="container ">
      <div class="">
        <h1 class="m-2 mb-4">LET'S KANGY !!</h1> <br>
        <div class="row p-2" style="display: flex;justify-content: center;">
          <div  v-for="i in parseInt(numberOfPlayer)" class="col-sm-12 col-md-6 p-3">
            <div @click="toggleModal(i-1)" class="player-card p-3">
           <h3>{{ this.playername[i-1] }}</h3><hr />
              <div class="row">
                <CTable :columns="columns" :items="getTableItems(i-1)" />
              </div>
          </div>
          </div>
        </div>
      </div>
    </div>

 <CModal 
 alignment="center"
    scrollable
    :visible="showModal"
    @close="() => { showModal = false }"
  >
    <CModalHeader>
      <CModalTitle>Pay to {{ playername[winner] }}</CModalTitle>
    </CModalHeader>
    <CModalBody>
      <div v-if="ace !== null && ace > 0" class="">
      <CButton @click="addAce(this.ace)" class="m-1" color="warning" style="color: white;">{{ aceToggled }} Ace</CButton>
      <CButton @click="clearAce()" class="m-1" color="danger" style="color: white;">Clear</CButton>
      </div>
      

      <template v-for="i in parseInt(numberOfPlayer)">
        <div v-if="i-1 !== winner" class="col-sm-12 col-md-6 p-3">
            <div class="player-card p-3">
              <div class="row">
                  <div class="col-4"><label><CFormCheck  v-model="checkboxModel[i-1]" />&nbsp; {{ this.playername[i-1] }}</label></div>
                  <div class="col-8">
                    <div class="d-flex justify-content-center">
                      <CButton @click="decrementInput(i-1)" color="primary">-</CButton>&nbsp;
                      <CFormInput v-model="inputModel[i-1]" style="width: 100px;" type="number" min="0"/>&nbsp;
                      <CButton @click="incrementInput(i-1)" color="primary">+</CButton> &nbsp;
                    </div>
                     
                  </div>
              </div>

          </div>
          </div>
      </template>
      
    </CModalBody>
    <CModalFooter>
      <CButton color="secondary" @click="() => { showModal = false }">
        Close
      </CButton>
      <CButton @click="savePayment()" color="primary">Save changes</CButton>
    </CModalFooter>
  </CModal>

  </div>
</template>

<script>
import '@/utils/kangy.css'
import { CButton, useColorModes, CModal } from '@coreui/vue'

export default {
  name: 'Dashboard',
  components: {

  },
  data(){
    return {
      activeButton:0,
      currentState:'0',
      numberOfPlayer:2,
      charge:1,
      aceToggled:0,
      ace:0,
      playername: new Array(),
      columns: [
          {
            key: 'name',
            _props: { scope: 'col' },
          },
          {
            key: 'money',
            label: 'Money',
            _props: { scope: 'col' },
          },
      
        ],
        showModal:false,
        winner:null,
        dataMetric:null,
        inputModel:null,
        checkboxModel:null,
       
  }
  },
  created(){
    this.currentState = localStorage.getItem("CurrentState") || '0'
    this.numberOfPlayer =  parseInt(localStorage.getItem("NumberOfPlayer")) || 2
    this.charge =  parseFloat(localStorage.getItem("Charge")) || null
    this.ace =  parseFloat(localStorage.getItem("Ace")) || null
    this.activeButton = this.numberOfPlayer
    this.playername = JSON.parse(localStorage.getItem("PlayerName")) || new Array()
    this.dataMetric = JSON.parse(localStorage.getItem("DataMetric")) || null
    this.inputModel = JSON.parse(localStorage.getItem("InputModel")) || null
    this.checkboxModel = JSON.parse(localStorage.getItem("CheckboxModel")) || null
    localStorage.setItem("coreui-free-vue-admin-template-theme","dark") //light
    const { colorMode, setColorMode } = useColorModes('coreui-free-vue-admin-template-theme')
    document.title = "Kangy"
  },
  computed:{    
  },
  methods:{
    addAce(value){
      if(this.aceToggled <=3){
        this.aceToggled += 1
        for (let index = 0; index < this.inputModel.length; index++) {
        this.inputModel[index] = this.inputModel[index] + parseFloat(value)
        }
      }
    },
    clearAce(){
      this.inputModel = JSON.parse(localStorage.getItem("InputModel"))
      this.aceToggled = 0
    },
    decrementInput(index) {
    if (this.inputModel[index] > 1) {
      this.inputModel[index]--;
    }
  },
  incrementInput(index) {
    this.inputModel[index]++;
  },
    saveState0(){
      if(this.charge <= 0){
        alert('Invalid Charge')
      }else{
        localStorage.setItem("Charge",this.charge)
        localStorage.setItem("Ace",this.ace)
        let data = []
        let model = []
        let checkbox = []

        for(let i=0;i<this.numberOfPlayer;i++){
          let subData = []
          for(let j=0;j<this.numberOfPlayer;j++){
            subData.push(0)
        }
        model.push(parseFloat(this.charge))
        checkbox.push(true)
        data.push(subData)
        }
        this.dataMetric = data
        localStorage.setItem("DataMetric",JSON.stringify(data))
        localStorage.setItem("InputModel",JSON.stringify(model))
        localStorage.setItem("CheckboxModel",JSON.stringify(checkbox))

        this.checkboxModel = checkbox
        this.inputModel = model
        window.pageYOffset = 0
        this.currentState = '1'
      }
     
    },
    setNumberPlayer(i){
      this.activeButton = i
      localStorage.setItem("NumberOfPlayer",i)
      localStorage.setItem("CurrentState",this.currentState)
      this.numberOfPlayer = parseInt(localStorage.getItem("NumberOfPlayer"))
    },
    savePlayer(){
      for(let i=0;i<this.numberOfPlayer;i++){
       if(this.playername[i] == undefined || this.playername[i] == ""){
          let number = i+1
          this.playername[i] = 'P'+ number
       }
      }
      localStorage.setItem("PlayerName",JSON.stringify(this.playername))

      this.currentState = '2'
      localStorage.setItem("CurrentState",this.currentState)
    },
    getTableItems(j){
      let item = [] 
        for(let i=0;i<this.numberOfPlayer;i++){
          if(i !== j){
            item.push( {
            id: i+1,
            name: this.playername[i],
            money: this.dataMetric[j][i],
            _cellProps: { id: { scope: 'row' } },
          } )
          }
        }
          return item
    },
    toggleModal(i){
      console.log(this.dataMetric)
      console.log(this.checkboxModel)
      this.checkboxModel = JSON.parse(localStorage.getItem("CheckboxModel"))
      this.inputModel = JSON.parse(localStorage.getItem("InputModel"))
      this.aceToggled = 0
      this.winner = i
      this.showModal = true
    },

    savePayment(){
      for(let i=0;i<this.numberOfPlayer;i++){
          for(let j=0;j<this.numberOfPlayer;j++){
           if(j == this.winner && this.checkboxModel[i] == true){
            this.dataMetric[j][i] += this.inputModel[i]
           }else{
            if(i == this.winner && this.checkboxModel[j] == true){
              this.dataMetric[j][i] -= this.inputModel[j]
            }
           }
        }
        }
        this.showModal = false
        localStorage.setItem("DataMetric",JSON.stringify(this.dataMetric))
    }



  },
 
  
}
</script>
<style>
</style>
