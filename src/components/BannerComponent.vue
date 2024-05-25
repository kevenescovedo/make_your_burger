<template>
   <div id="main-banner">
    <h1>Faça seu Burger</h1>
   </div>
</template>
<script>
export default {
name: 'BannerComponent',
}
</script>

<style scoped>
#main-banner {
   background-image: url('../../public/img/burger.jpg');
   background-position: center;
   background-size: cover;
   height: 500px;
   display: flex;
   align-items: center;
   justify-content: start;

}
#main-banner h1 {
    color: #fff;
    font-size: 60px;
    text-align: center;
    background-color: #222;
    padding: 20px 40px;
}
</style>