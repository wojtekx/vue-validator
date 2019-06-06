<template>
  <div class="app">
    <form class="form" @submit.prevent="send">
      <p>Wybierz rodzaj zamówienia:</p>
     <select name="type" v-model="type" >
       <option value="usluga">Usługa</option> 
       <option value="produkt">Produkt</option> 
     </select>
      <span v-show="type=='usluga' && (value < 10000 || value > 50000) && email">Nieporpawna wartość! Wprowadz od 10000 do 50000</span>
      <span v-show="type=='produkt' && (value < 1|| value > 50) && email ">Nieporpawna wartość! Wprowadz od 1 do 50</span>
     <input type="number" v-if="!type" disabled >
     <input type="number" name="value" min="1" max="50" v-else-if='type=="produkt"' v-model="value" required placeholder="Wartość od 1 do 50">
     <input type="number" name="value" min="10000" max="50000" v-else-if="type=='usluga'" v-model="value" required placeholder="Wartość od 10000 do 50000">
     <input type="email" disabled v-show="!value"  >
     <input type="email" name="email" v-model="email" v-show="value" required placeholder="Wpisz swój email" >
     <div class="radioInputs">
       <b>Zgoda na cokolwiek:</b>
      <input type="radio" name="booleanValue" value="true" id="yes" v-model="booleanValue" ><label for="yes">Yes</label>
      <input type="radio" name="booleanValue" value="false" id="no" v-model="booleanValue"><label for="no">No</label>
     </div>
     <b>Wiadomość:</b>
     <textarea v-if="!email" disabled></textarea>
     <textarea  name="note" minlength="25" v-if="email" v-model="note"></textarea>
     <small>Wszystkie pola muszą być wypełnione przed wysłaniem formularza!</small>
     <button v-if="!disableBtn" disabled >Wyślij</button>
     <button v-else-if="disableBtn" > Wyślij</button>
      
    </form>
    <br/>
    <div id="result"></div>
  </div>
</template>

<script>
export default {
  name: "Form",
  data(){
    return{
      type:"",
      value:"",
      email:"",
      booleanValue:"true",
      note:"",
      disableBtn: false,
    }
  },
  methods:{
    send(){
      const result = {
        type: this.type,
        value:this.value,
        email:this.email,
        booleanValue:this.booleanValue,
        note:this.note
      }

      if(this.disableBtn){
        console.log(result)
        document.getElementById('result').innerHTML = `
        <div>
          <p>Typ Zamówienia: ${result.type}</p>
          <p>Wartość: ${result.value}</p>
          <p>Zgoda: ${result.booleanValue}</p>
          <p>Wiadomość: ${result.note}</p>
          <p>Email: ${result.email}</p>
        </div>` 
      }
    },
    
  },
  updated(){
        if( this.email && this.note.length > 50 && ((this.type=='usluga' && (this.value >= 10000 && this.value <= 50000)) || (this.type=='produkt' && (this.value >= 1 && this.value <= 50) ) )){
            this.disableBtn = true
      }else this.disableBtn = false
  }
}
</script>

<style scoped>
#result{
    text-align: left;
    width: 30%;
    margin: 0 auto;
}
#result p{
  word-break: break-word;
}
form{
    display: flex;
    flex-direction: column;
    width: 30%;
    margin: 0 auto;
}
span{
  color: red;
  margin-top: 11px;
  background-color: white;
  padding: 13px 2px;
  border: 1px solid red;
}
input, textarea {
    font-size: 20px;
    padding: 10px;
    margin: 10px 0;
}
select{
    height: 40px;
    font-size: 20px;
}
.radioInputs {
    display: flex;
    justify-content: start;
    align-items: center;
}
input[type="radio"] {
    margin-left: 20px;
}
button{
  width: 75px;
  height: 35px;
  background-color: #4457ce;
  border-radius: 10px;
  font-size: 19px;
  color: white;
  margin: 20px auto;
}
button:hover{
  cursor: pointer;
}
button:disabled{
  width: 75px;
  height: 35px;
  background-color: #666;
  border-radius: 10px;
  font-size: 19px;
  color: white;
  cursor: inherit;
  margin: 20px auto;
}
</style>
