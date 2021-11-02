<template>
    <div class="box-container">
        <div class="box">

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