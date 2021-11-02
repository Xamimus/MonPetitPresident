<template>
    <div class="box-container">
        <div class="box">
            <div class="box-header">
                <h1>Mon premier titre</h1>
            </div>
            <div class="box-content">
                Mon premier contenu
            </div>
            <div class="box-footer">
                <a class="btn btn-primary" href="#">Puis un bouton qui fait rien</a>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .box-container{
        min-height: 95vh;
        display: flex;
        justify-content: center;
        align-items: center;    
    }

    .box{
        background-color: #fff;
        height: 60vh;
        width: 45vw;
        border-radius: 15px;
        box-shadow: 5px 5px 5px rgb(48, 48, 48);
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
    }

    .box-header h1{
        
    }

    @media only screen and (max-width: 600px) {
        .box {
            height: 50vh;
            width: 80vw;
        }
    }

    @media only screen and (min-width: 2000px) {
        .box {
            height: 50vh;
            width: 40vw;
        }
    }
</style>