<template>
  <div class="modal-backdrop">
    <div class="modal">
      <header class="modal-header">
        <slot name="header">
          {{Title}}
        </slot>
      </header>

      <section class="modal-body">
        <slot name="body">
          {{Content}}
        </slot>
       </section>

      <footer class="modal-footer">
        <button
          type="button"
          class="btn-green"
          @click="close"
        >
          Fermer X
        </button>
      </footer>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Modal',
    Title: '',
    Content: '',
    methods: {
      close() {
        this.$emit('close');
      },
    },
  };
</script>

<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    opacity: 1;
    background-color: rgba(0, 0, 0, 0.5);
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: center;
    z-index: 10;
  }

  .modal {
    background-color: rgb(255, 255, 255);
    box-shadow: 2px 2px 20px 1px;
    position: relative;
    display: flex;
    flex-direction: column;
    height: 65vh;
    width: 45vw;
    justify-self: center;
    z-index: 11;
    opacity: 1;
  }

    .modal-header {
    padding: 15px;
    display: flex;
    justify-self: center;
    text-align: center;
    align-self: center;
  }

  .modal-footer {
    padding: 15px;
    display: flex;
    justify-self: end;
    text-align: right;
    align-self: end;
  }

  .modal-header {
    position: relative;
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    flex-direction: column;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 20px;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }
</style>