<template>
    <div class="footer-container" id="app">
        <button class="credits-container" @click="showModal">
            © {{new Date().getFullYear()}} | Mon Petit Président
        </button>
        <button class="legal-container" @click="showModal">
            Mentions Légales | Politique de confidentialité
        </button>  
        <Modal
        v-show="isModalVisible"
        @close="closeModal"
        >
        <template v-slot:header>
            This is a new modal header.
        </template>

        <template v-slot:body>
            This is a new modal body.
        </template>

        <template v-slot:footer>
            This is a new modal footer.
        </template>
        </Modal>
    </div>
</template>

<script>
import Modal from './Modal.vue';
export default {
    name: 'App',
    components: {
      Modal,
    },
    data() {
      return {
        isModalVisible: false,
      };
    },
    methods: {
      showModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      }
    }
}
</script>

<style scoped>
.footer-container{
    width: 100vw;
    background-color: #fff;
    height: 5vh;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
}

button{
    background-color: transparent;
    border: none;
}
</style>