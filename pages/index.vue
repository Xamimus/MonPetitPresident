<template>
  <div>
    <Box />
    <Footer :method="showModal" />
    <Modal
    v-show="isModalVisible"
    @close="closeModal"
    >
    <template v-slot:header>
        {{MyTitle}}
    </template>

    <template v-slot:body>
        {{MyContent}}
    </template>
    </Modal>
  </div>
</template>

<script>
import Modal from '../components/Modal.vue';
import app from '../styles/app.css'
/* Vue.component('myApp', {
  methods: {
    showModal(Title, Content) {
      this.isModalVisible = true,
      this.MyTitle = Title,
      this.MyContent = Content;
    },
  }
})  */
export default {
    style: app,
    components: {
      Modal,
    },
    data() {
      return {
        isModalVisible: false,
        MyTitle: '',
        MyContent: '',
      };
    },
    mounted() {
      this.$fire.firestore
      .collection('questions')
      .doc('Hk7ctzR5hbzEwq2BOoIZ')
      .get()
      .then((e) => console.log(e.data()))
    },
    methods: {
      showModal(Title, Content) {
        this.isModalVisible = true,
        this.MyTitle = Title,
        this.MyContent = Content;
      }, 
      closeModal() {
        this.isModalVisible = false;
      }
    }
}
</script>

