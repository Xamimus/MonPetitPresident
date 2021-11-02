<template>
  <div class="box-container">
    <div class="row">
      <div class="col"><div class="polygonblue"></div></div>
      <div class="col">
        <div class="box">
          <div class="row"></div>
          <div class="row"></div>
        </div>
      </div>
      <div class="col"><div class="polygonred"></div></div>
    </div>
  </div>
</template>



<style scoped>
.box-container {
  min-height: 95vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.box {
  background-color: #fff;
  height: 60vh;
  width: 45vw;
  border-radius: 15px;
  box-shadow: 5px 5px 5px rgb(48, 48, 48);
}
.polygonblue {
  -webkit-clip-path: polygon(0 0, 0% 100%, 100% 0);
  clip-path: polygon(0 0, 0% 100%, 100% 0);
  background-color: #002395;
  box-shadow: 5px 4px 4px rgba(0, 0, 0, 0.25);
  width: 110px;
  height: 97px;
  position: absolute;
  box-shadow: 5px 5px 4px rgba(0, 0, 0, 0.25);
  left: 40px;
  top: -5px;
  z-index: 2;
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
.polygonred {
  -webkit-clip-path: polygon(100% 100%, 0% 100%, 100% 0);
  clip-path: polygon(100% 100%, 0% 100%, 100% 0);
  background-color: #ed2939;
  position: absolute;
  box-shadow: 5px 5px 4px rgba(0, 0, 0, 0.25);
  width: 110px;
  height: 97px;
  bottom: -5px;
  right: 40px;
}

@media only screen and (max-width: 600px) {
  .box {
    height: 50vh;
    width: 80vw;
  }
  .polygonblue, .polygonred {
      display: none;
  }
}

@media only screen and (min-width: 2000px) {
  .box {
    height: 50vh;
    width: 40vw;
  }
}
</style>
