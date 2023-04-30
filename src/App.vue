<template>

  <main>
    <h1>Welcome to csv2json!!</h1>

    <div class="switches">
      <button @click="switchConv">{{ this.toJson ? "convert CSV to JSON" : "convert JSON to CSV" }}</button>
    </div>
    

    <form @submit.prevent="validateCsv" v-show="toCsv">
      <textarea name="inCsv" v-model="inputData" required placeholder="Type the CSV here..."/>
      <button type="submit">convert!</button>
    </form>
    <form @submit.prevent="validateJson" v-show="toJson">
      <textarea name="inJson" v-model="inputData" required placeholder="Type the JSON here..."/>
      <button type="submit">convert!</button>
    </form>
    <div v-show="done" class="result">
    <h3>The json is:</h3>
    <br>
    <span>{{ result }}</span>
    </div>


  </main>
  


</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      toJson:false,
      toCsv:true,

      inputData:'',
      csvArr:null,

      totalDataRows:0,
      attrs:null,

      result:[],
      done: false
    }
  },
  methods:{
    switchConv(){
      this.toJson = !this.toJson
      this.toCsv =  !this.toCsv
    },
    validateCsv(){
      let trimCsvArr = this.inputData.trim()
      this.csvArr = trimCsvArr.split(/\n/)
      this.attrs = this.csvArr.splice(0,1)
      
      let attrsValues = this.attrs[0].split(',')
      
      console.log(attrsValues, this.csvArr)
      this.csvArr.forEach((row, index)=>{
        let rowData = row.split(',')
        
        this.totalDataRows += rowData.length 
        console.log(index)
      })

      if(this.totalDataRows % attrsValues.length == 0){
        this.convertCsv(attrsValues)
      }else{
        alert("Is not a valid Csv")
        this.clearCSV()
      }

    },
    validateJson(){
      let trimJson = this.inputData.trim()
      
      try{
        JSON.parse(trimJson)
      }catch(e){
        window.alert("Is not a a valid JSON")
      }
      
    },
    convertCsv(attrsValues){
      this.result = []
      this.csvArr.map(row=>{
        var rowData = row.split(',')
          attrsValues.map((headingAttr, index)=>{
            var objToPush = new Object()

            objToPush[headingAttr] = rowData[index]

            this.result.push(objToPush)
          })

      })
      this.done = true
      
      this.clearCSV()
      console.log(this.result)

     },
    clearCSV(){
        this.csvArr=null,
        this.totalDataRows=0,
        this. attrs=null
    },
    
  }
  
}
</script>

<style>
*{
  margin:0;
  padding:0;

  box-sizing: border-box;
}

html,body{

  width:100%;
  height: 100%;

  font-family: Helvetica, sans-serif;

  background-image: linear-gradient(to right top, #6bd1a9, #74d6af, #7ddbb5, #86e0bc, #8fe5c2, #9ae8ca, #a5ecd1, #b0efd8, #c1f2e1, #d1f4e9, #e3f6f1, #f4f8f7);

}
main, form{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  margin-top: 48px;
}
button{
  width: 280px;
  height: 48px;

  margin-top: 24px;
  border:0;
  border-radius: 4px;
  background: #fff;

  font-size: 16px;
  
  transition: .5s;

  cursor:pointer;

}
button:hover{
  background: transparent;
  border: 1px solid #000;

}
textarea{
  width: 1024px;
  height: 240px;

  padding: 8px;
}
.result{
  width: 860px;
  

  background: #FFF;
  color:#221D23;
  font-size: 18px;

  margin-top: 36px;

  padding: 16px;

  border:0;
  border-radius: 8px;
}
.result h3{
  color: #4F3824;
}
</style>
