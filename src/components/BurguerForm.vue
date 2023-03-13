<template>
  <div>
    <Message :msg="msg" v-show="msg" />
  </div>

  <div>
    <form id="burger-form" @submit="postBurguer">

      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o nome do cliente">
      </div>

      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select id="pao" name="pao" v-model="pao">
          <option :value="null">Selecione o seu pão...</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
        </select>
      </div>

      <div class="input-container">
        <label for="carne">Escolha o seu bife:</label>
        <select id="carne" name="carne" v-model="carne">
          <option :value="null">Selecione o tipo de bife:</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
        </select>
      </div>

      <div class="opcionais-container input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
          <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.id">
            <label class="label-opcional">
            <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
            <span>{{ opcional.tipo }}</span>
            </label>
          </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="Montar meu Lanche">
      </div>

    </form>
  </div>
  
</template>

<script>
  import Message from "./Message.vue"

  export default{
    name: "BurguerForm",
    data()
    {
      return{
        nome: null,
        paes: null,
        carnes: null,
        opcionaisData: null,
        pao: null,
        carne: null,
        opcionais: [],
        msg: null
      }
    },
    components: {
      Message
    },
    methods: {
     async getIngredientes(){       
        const response = await fetch("http://localhost:3000/ingredientes")
        const data = await response.json()
        this.paes = data.paes
        this.carnes = data.carnes
        this.opcionaisData = data.opcionais

        console.log(data)
      },

      async postBurguer(e){
        e.preventDefault()

        const data = {
          nome: this.nome,
          pao: this.pao,
          carne: this.carne,
          opcionais: Array.from(this.opcionais),
          status: "Solicitado"
        }

        const dataJson = JSON.stringify(data)

        const response = await fetch("http://localhost:3000/burgers", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: dataJson
        })

        const res = await response.json()

        console.log(res)

        //mostrar mensagem de sucesso
        this.msg = `Pedido Nº ${res.id} realizado com sucesso!`

        //limpar mensagem de sucesso
        setTimeout(() => {
          this.msg = ""
        }, 3000)

        //limpar os campos
        this.nome = ""
        this.pao = null
        this.carne = null
        this.opcionais = ""

      }

    },
    mounted(){
      this.getIngredientes()
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

label{
  font-size: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #FCBA03;
}

.label-opcional{
  font-size: bold;
  margin-bottom: 5px;
  color: #222;
  padding: 5px 10px;
  border-left: none;
  cursor: pointer;
}

input, select {
   padding: 5px 10px;
   width: 300px;
}

#opcionais-container{
  flex-direction: row !important;
  flex-wrap: wrap !important;
}

#opcionais-title{
  width: 100%;
}

.checkbox-container{
  display: flex;
  align-items: flex-start;
  width: 50px;
  margin-bottom: 5px;
}

.checkbox-container,
.checkbox-container input{
  width: auto;
}

.checkbox-container span{
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn{
  background-color: #222;
  color: #FCBA03;
  border: 2px solid #222;
  padding: 10px;
  border-radius: 5px;
  font-size: 16px;
  font-weight: bold;
  margin: 0 auto;
  cursor: pointer;
  transition: .5s;
}

.submit-btn:hover{
  background-color: transparent;
  color: #222;
}

</style>