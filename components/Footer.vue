<template>
    <div class="footer-container">
        <button class="credits-container">
            © {{this.date}} | Mon Petit Président
        </button>
        <button class="legal-container">
            Mentions Légales | Politique de confidentialité
        </button>  
    </div>
</template>

<script>
export default {
    data: () => ({
        date: new Date().getFullYear(),
  }), 
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