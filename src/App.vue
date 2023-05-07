<template>

  <main>
    <h1>Welcome to csv2json!</h1>
    <p>Convert your csv to json or json to csv, just paste the content or attach the corresponding file.</p>

    <div>
      <button class="toogleButton" @click="switchConv">{{ this.toJson ? "CSV to JSON" : "JSON to CSV" }}</button>
    </div>
    <div class="content">
      <div class="forms">
    <form @submit.prevent="validateCsv" v-show="toCsv">
      <textarea name="inCsv" v-model="inputData" required placeholder="Type the CSV here..."/>
      <div class="buttons">
        <button type="submit" class="convertButton">convert!</button>
      </div>
    </form>
    <form @submit.prevent="validateJson" v-show="toJson">
      <textarea name="inJson" v-model="inputData" required placeholder="Type the JSON here..."/>
      <div class="buttons">
        <button type="submit" class="convertButton">convert!</button>
      </div>
    </form>
      </div>
    <div class="result">
    <span>{{ result }}</span>
    </div>

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

      result:null,
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
      
      console.log(trimCsvArr, attrsValues, this.csvArr)

      this.csvArr.forEach((row)=>{
        let rowData = row.split(',')
        
        this.totalDataRows += rowData.length 
        
      })
        console.log(this.totalDataRows, attrsValues.length, this.csvArr)
        console.log(this.totalDataRows%attrsValues.length)
        
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

  font-family: 'Poppins', sans-serif;

  background: #FEFAE0;

}

main{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  
}
h1{
  margin-top: 48px;
}
.content{
  display: flex;
  margin-top: 48px;

}
.forms{
  display: flex;
  align-items: center;
}
.forms form{
  display: flex;
}
.buttons{
  display: flex;
  flex-direction: column;

  padding-top: 240px;
}

button{
  width: 120px;
  height: 48px;

  margin-top: 24px;
  border:0;
  border-radius: 8px;

  font-size: 16px;
  
  transition: .5s;

  cursor:pointer;

}
.toogleButton{
  background-color: #CCD5AE;
  font-family: 'Poppins', sans-serif;
  font-weight: 500;
}
.convertButton{
  font-family: 'Poppins', sans-serif;
  font-weight: 500;
  background-color: #D4A373;

  margin: 0 24px;

}
button:hover{
  background: transparent;
  border: 1px solid #000;

}

textarea, .result{

  font-size: 12apx;
  padding: 8px;

  background: #FAEDCD;
  border: 2px solid #CCD5AE;
  border-radius: 8px;

  width: 640px;
  height: 520px;
}

textarea::placeholder{
  color:#717171;
}

.result h3{
  color: #4F3824;
}

@media screen and (max-width:1366px){
  textarea, .result{
  padding: 8px;

  background: #FAEDCD;
  border: 2px solid #CCD5AE;
  border-radius: 8px;

  width: 320px;
  height: 260px;
}
.buttons{
  padding-top: 104px;
}
}
</style>
