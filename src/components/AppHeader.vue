<template>
  <CHeader position="sticky" :class="headerClassNames">
    <CContainer class="border-bottom px-4" fluid>
      <CHeaderToggler @click="$store.commit('toggleSidebar')" style="margin-inline-start: -14px">
        <CIcon icon="cil-menu" size="lg" />
      </CHeaderToggler>
      <CHeaderNav>
        <CButton  @click="showModal()" color="success" style="color: white;">Contact Me ☎️</CButton> &nbsp;
        <CButton @click="clearLocalStorage()" color="danger" style="color: white;">Clear</CButton>
    
      </CHeaderNav>
    </CContainer>
    <CModal 
    :visible="modalVisible"
    @close="() => { modalVisible = false }"
  >
    <CModalHeader>
      <CModalTitle >Hi! I'm Coke</CModalTitle>
    </CModalHeader>
    <CModalBody>
      <div class="d-flex justify-content-center">
        <img style="border-radius: 50%;" width="40%" src="/src/assets/images/img/me.jpg">
      </div>
      <div class="text-center mt-4">
        <h3>Akkaradet Wittayawuttirat</h3>
        <span>Front-end Developer</span>
      </div>
      <div class="mt-4">
        <CButton @click="pushLocation('https://github.com/Cokeizz')" class="mt-2" style="width: 100%;font-weight: bold;" color="light"> <img src="https://cdn-icons-png.flaticon.com/512/2111/2111425.png " width="20" height="20" alt="" title="" class="img-small mb-1"> Github</CButton>
        <CButton @click="pushLocation('https://www.linkedin.com/in/akkaradet-wittayawuttirat-635082252')" class="mt-2" style="width: 100%;font-weight: bold;" color="light"> <img src="https://cdn-icons-png.flaticon.com/512/145/145807.png " width="20" height="20" alt="" title="" class="img-small mb-1"> LinkedIn</CButton>
        <CButton @click="showEmail = !showEmail" class="mt-2" style="width: 100%;font-weight: bold;" color="light"> <img src="https://cdn-icons-png.flaticon.com/512/9068/9068642.png " width="20" height="20" alt="" title="" class="img-small mb-1"> Email</CButton>
        <CButton v-if="showEmail" class="mt-1" style="width: 100%;font-weight: bold;opacity: 0.7;" color="light">akkaradet.akrd@gmail.com</CButton>
        <CButton @click="pushLocation('https://www.instagram.com/cokeizz/')" class="mt-2" style="width: 100%;font-weight: bold;" color="light"> <img src="https://cdn-icons-png.flaticon.com/512/15707/15707749.png " width="20" height="20" alt="" title="" class="img-small mb-1"> Instagram</CButton>
      </div>
      <CCallout color="info">
                  Hey there,<br><br>
                  Hey, I'm Coke, a Computer Science grad from Khon Kaen University, currently a front-end developer at Silom Technology. Passionate about indie game development, I even made one for my college project. When not coding, I'm into drawing cartoons and pixel art.
          <br> Last but not least!,  I'm a BIG SWIFTIE🎸✨
</CCallout>
  </CModalBody>
    <CModalFooter>
      <CButton color="secondary" @click="() => { modalVisible = false }">
        Close
      </CButton>
    </CModalFooter>
  </CModal>
  </CHeader>
</template>

<script>
import { onMounted, ref } from 'vue'
import { CButton, useColorModes } from '@coreui/vue'
import AppHeaderDropdownAccnt from './AppHeaderDropdownAccnt'
import { cilShower } from '@coreui/icons'
export default {
  name: 'AppHeader',
  components: {
    AppHeaderDropdownAccnt,
  },
  data(){
    return {
      modalVisible:false,
      showEmail : false,
    
    }
  },
  setup() {
    const headerClassNames = ref('mb-4 p-0')
    const { colorMode, setColorMode } = useColorModes('coreui-free-vue-admin-template-theme')

    onMounted(() => {
      document.addEventListener('scroll', () => {
        if (document.documentElement.scrollTop > 0) {
          headerClassNames.value = 'mb-4 p-0 shadow-sm'
        } else {
          headerClassNames.value = 'mb-4 p-0'
        }
      })
    })

    return {
      headerClassNames,
      colorMode,
      setColorMode,
    }
  },
  methods:{
  clearLocalStorage(){
    localStorage.clear()
    window.location.reload()
  },
  showModal(){
    this.modalVisible = true
  },
  pushLocation(url){
    window.open(url, '_blank');
  }
},
}
</script>

