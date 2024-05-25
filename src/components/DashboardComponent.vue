<template>
    <div>
    <div id="burger-table" v-if="burgers">

        <MessageComponent :msg="msg" v-show="msg"/>

        <div id="burger-table-heading">
            <div class="order-id">#:</div>
            <div>Cliente:</div>
            <div>Pão:</div>
            <div>Carne:</div>
            <div>Opcionais:</div>
            <div>Ações:</div>
        

        </div>
        <div id="burger-table-rows">
            <div id="burger-table-row"  v-for="(burger) in burgers" :key="burger.id">
                <div class="order-number">{{ burger.id}}</div>
                <div>{{burger.nome}}</div>
                <div>{{burger.pao}}</div>
                <div>{{burger.carne}}</div>

                <div>
                        <ul v-for="(opcional,index) in burger.opcionais" :key="index">
                      
                        <li>{{ opcional }}</li>
                    </ul>
               </div>
             
                    <div>

                            <select name="status" class="status" @change="updatedBurger($event, burger.id)">
                                <option value="">Selecione</option>
                                <option v-for="sta in status" :key="sta.id" :value="sta.tipo" :selected="burger.status === sta.tipo">{{ sta.tipo }}</option>


                            </select>
                            <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar </button>
                    </div>
          
           </div>
        </div>
   
    </div>
    <div v-else>Não há pedidos no momento!</div>
</div>
</template>

<script>
import MessageComponent from './MessageComponent.vue';

export default {
    name: 'DashboardComponent',
    components: {
        MessageComponent
    },
    data() {
        return {
            burgers: null,
            burger_id: null,
            status : [],
            msg: null,
        }
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://192.168.0.11:3000/burgers");
            const data = await req.json();
            this.burgers = data;
              
            //resgatar os status

        },
        async getStatus() {
             const req = await fetch("http://192.168.0.11:3000/status");
             const data = await req.json();
             this.status = data;
             console.log(data);
        },
        async deleteBurger(id) {
            const req = await fetch(`http://192.168.0.11:3000/burgers/${id}`, {
                method: "DELETE",
            });
        const res = await req.json();

        this.getPedidos();
        this.msg = "Pedido excluido com sucesso!!";
        setTimeout(() => this.msg = "", 3000);

            console.log(id);
        },
        async updatedBurger(event,id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({status: option});
            const req = await fetch(`http://192.168.0.11:3000/burgers/${id}`, {
                method: 'PATCH',
                headers: {"Content-Type": "application/json"},
                body: dataJson


            });

            const res = await req.json();

            this.msg = `O Pedido de número ${res.id} foi atualizado para ${res.status}`;
            setTimeout(() => this.msg = "", 3000);



        }

    },
    mounted() {
        this.getPedidos();
        this.getStatus();
    }
}

</script>
<style scoped>
#burger-table {
    max-width: 1200px;
    margin: 0 auto;

}
#burger-table-heading,

#burger-table-row {
    display: flex;
    flex: wrap;
}
#burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom:  3px solid #333;
}
#burger-table-heading div,
#burger-table-row div {
    width: 19%;
}
#burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #ccc;

}
#burger-table-heading .order-id ,
#burger-table-row .order-number {
    width: 5%;
}
select {
    padding: 12px 6px;
    margin-right: 12px;
}
.delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
    transition: .5s;

}
.delete-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>


