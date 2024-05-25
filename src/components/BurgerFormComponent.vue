<template>
    <div>
       <MessageComponent :msg="msg" v-show="msg"/>
        <form id="burger-form" @submit="createBurger">
        <div class="input-container">
            <label for="nome">Nome do Cliente:</label>
            <input type="text" name="nome" id="nome" v-model="nome" placeholder="Digite o seu nome">
        </div>

        <div class="input-container">
            <label for="pao">Escolha o pão:</label>
            <select name="pao" id="pao" v-model="pao">
                <option value="" :selected=true>Selecione o seu pão</option>
                <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{pao.tipo }}</option>
            </select>
           
        </div>

        <div class="input-container">
            <label for="carne">Escolha a carne do seu burger:</label>
            <select name="carne" id="carne" v-model="carne">
                <option value="" :selected=true>Selecione o tipo da carne</option>
                <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
            </select>
           
        </div>

        <div class="input-container">
            <label for="opcionais-title" id="">Selecione os opcionais:</label>
            <div id="opcionais-container" class="input-container">
                <div v-for="opcional in opcionaisdata" :key="opcional.id" class="checbox-container">
                <input type="checkbox" name="opcionais" id="opcionais" v-model="opcionais"  :value="opcional.tipo">
                <span>{{ opcional.tipo }}</span>
                </div>
                

            </div>
           
        </div>

        <div class="input-container">
           <input type="submit" class="submit-btn" value="Criar meu burger">
           
        </div>

        </form>
    </div>
</template>
<script>
import MessageComponent from './MessageComponent.vue';

export default {
   name: 'BurgerFormComponent',
   data() {
    return {
       paes: null,
       carnes: null,
       opcionaisdata: null,
       nome: null,
       pao: "",
       carne: "",
       opcionais: [],
       status: "Solicitado",
       msg: null 
    }
  
   },
   methods: {
    async getIngredientes() {
           const req = await fetch('http://192.168.0.11:3000/ingredientes');
           const data = await req.json();
           console.log(data);
           this.paes = data.paes;
           this.carnes = data.carnes;
           this.opcionaisdata = data.opcionais;
    },
    async createBurger (e) {
       e.preventDefault();
       const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
       }
       console.log(this.opcionais);


       console.log(data);
       const dataJson =  JSON.stringify(data);
       const req = await fetch('http://192.168.0.11:3000/burgers', {
        method: "POST",
        headers: {
            "Content-Type": "application/json",

        },
        body: dataJson
       })
       const res = await req.json();
       
       //mostrar menssagem
       this.nome = "";
       this.carne = "";
       this.pao = "";
       this.opcionais = "";
       this.msg = "Pedido realizado com sucesso!!";

       setTimeout(() => this.msg = "", 3000);


    }
      
  },
  mounted() {
    this.getIngredientes();
  },
  components: {
    MessageComponent
  }
}
</script>
<style scoped>
#burger-form {
    max-width: 400px;
    margin: 0 auto;
}
.input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}
label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #FCBA03;

}

input, select {
    padding: 5px 10px;
    width: 300px;
}
#opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
}
#opcionais-title {
width: 100%;
}
.checbox-container { 
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}
.checbox-container span, .checbox-container input {
    width: auto;
}
.checbox-container span {
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
    transition: .5s;
}
.submit-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>
