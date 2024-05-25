<template>
    <div id="nav">

        <router-link to ="/"> <img :src="logo" :alt="alt" id="logo"></router-link>

        <nav>
          
            <router-link to="/">Home</router-link> 
            <router-link to="/pedidos">Pedidos</router-link>
    </nav>
    </div>
</template>

<script>
export default {
    name: 'NavbarComponent',
    props:['logo', 'alt']

}
</script>
<style scoped>
#nav {
    background-color: #222;
    border-bottom: 4px solid #111;
    padding: 15px 50px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    /*justify-content: flex-end;
    align-items: center;*/
}
#logo {
    height: 40px;
    width: 40px;
}
#nav a {
    color:#FCBA03;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
}
#nav a:hover {
    color: white;
}
</style>
