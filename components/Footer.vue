<template>
    <div class="footer-container">
        <span class="credits-container">
            © {{new Date().getFullYear()}} | Mon Petit Président
        </span>
        <div>
            <button class="legal-container" @click="method('Mentions Légales', 'Content de Mentions Légales | Politique de confidentialité')">
                Mentions Légales
            </button>  
            <span>|</span>
            <a href="../files/Politique_de_confidentialite.pdf" download="Politique_de_confidentialite.pdf">
                <button type="button">Politique de confidentialité</button> 
            </a>
        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    props: ["method"],
    created() {
        // Use the parent function directly here
        //this.method();
    },
    methods: {
        showModal(Title, Content) {
            this.method();
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