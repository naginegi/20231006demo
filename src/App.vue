<script>
import HelloWorld from "./components/helloworld.vue";
import Dialog from "./components/dialog.vue";
export default {
  data() {
    return {
      balance: 0,
      incom:0,
      incomarr: [],
      expense:0,
      expensearr: [],
      staus : true,
      arr: [],
      dialogVisible: false ,
      newname:" ",   //輸入的名 text
      newnumber:" ", //輸入的錢 number
      userData: '',
      savedData: ''
      //money:Math.floor(Math.random()*100+1)
    };
  },
  mounted(){
      const savedTransactions = localStorage.getItem("userData");
      // const incomcalcu = localStorage.getItem("userincom");
      if(savedTransactions){
        this.arr = JSON.parse(savedTransactions);
        // this.arr = JSON.parse(incomcalcu);
      }
      this.calculate();
    },
  methods: {
    removeTask(index) {
      this.arr.splice(index, 1); // 移除指定索引的任务
      localStorage.setItem('userData',JSON.stringify(this.arr))
      // this.calculate(obj.money);
      this.calculate();
    },
    t() {
      console.log(this.arr)
      //arr.splice(arr.length - 1, 1);
      //console.log(this.arr);
      //this.balance = this.incom - this.expense;
      //console.log(Math.floor(Math.random())*2+1);
      // this.staus = Math.random() < 0.5;
      // console.log(this.staus)
    },
    
    // saveData() {
    //   // 使用 localStorage 保存数据
    //   localStorage.setItem('userData', this.userData);
    //   this.userData = ''; // 清空输入框
    // },
    // getData() {
    //   // 使用 localStorage 获取保存的数据
    //   this.savedData = localStorage.getItem('userData');
    // },
    addTask() {
      let obj = {
        name: this.newname,
        money: this.newnumber,
      };
      this.arr.push(obj);
      localStorage.setItem('userData',JSON.stringify(this.arr));
      this.calculate();
      this.newname = ''; // 清空输入框
      this.newnumber = ''; // 清空输入框
      console.log(this.arr);
    },

    calculate(){
      this.incom =0;
      this.expense=0;
      for(let i = 0 ; i<this.arr.length ; i++){
        if(this.arr[i].money >= 0){

          this.incom = this.incom+this.arr[i].money
        }else{
          this.expense =this.expense+this.arr[i].money
        }        
        // console.log(this.arr[i].money);
        // console.log(this.arr[i].money)
      }
      console.log("in"+this.incom)
      console.log("ex"+this.expense)
      this.balance = this.incom + this.expense

      // if(m<0){
      //   this.expensearr.push(m)
      //   // localStorage.setItem('userexpense',JSON.stringify(this.expensearr));
      //   // console.log("ex:"+this.expense)
      // }
      // if(m>=0){
      //   this.incomarr.push(m)
      //   // localStorage.setItem('userincom',JSON.stringify(this.incomarr));
      //   // console.log("in:"+this.incom)
      // }
      // incomcalcu = localStorage.getItem("userincom");
      // for(let i=0;i<this.incomcalcu.length;i++){
      //   this.incom = this.incom + this.incomcalcu[i]
      // }
      
      // localStorage.setItem('userData',this.arr)
      //localStorage.setItem('userNumber',this.newnumber)
      
      // if (this.newTask.trim() !== '') {
      //   this.arr.push(this.newTask);
      //   this.newTask = ''; // 清空输入框
      // }
    },
    cool() {
      console.log(this.dialogVisible)
      let obj = {
        name: "incom",
        money: Math.floor(Math.random() * 100 + 1),
      };
      let obj2 = {
        name: "expense",
        money: Math.floor(Math.random() * 100 + 1) * -1,
      };
      
      if(this.staus = Math.random() < 0.5){
        this.staus = true;
        this.arr.push(obj);
      }else{
        this.staus = false; 
        this.arr.push(obj2);
      }
      console.log(this.arr)
      this.incom = this.incom + obj.money;
      this.expense = this.expense + obj2.money;
      this.balance = this.incom + this.expense;
    },
    openDialog() {
      this.dialogVisible = true; // 打开对话框
    },
    closeDialog() {
      this.dialogVisible = false; // 关闭对话框
    }

  },
  components: {
    HelloWorld,
    Dialog
  },
};
</script>

<template>
  <div class="area">
    <!-- 左半部藍色區塊 -->
    <div class="left">
      <h1 class="title">Expense Tracker</h1>
      <h1 class="name">name</h1>
      <h3 class="h5">YOUR BALANCE</h3>
      <h1 class="balance" id="blance">{{ balance }}</h1>
    </div>
    <!-- 右半部白色區塊 -->
    <div class="right">
      <!-- 右上，包含收入、支出 -->
      <div class="top">
        <div class="incom">
          <h1 class="text">INCOM</h1>
          <h2 class="money" id="incom-money">{{ incom }}</h2>
        </div>
        <div class="expense">
          <h1 class="text">EXPENCE</h1>
          <h2 class="money" id="expense-money">{{ expense }}</h2>
        </div>
      </div>
      <!-- 右下，按鈕和顯示區塊 -->
      <div class="bottom">
        <button type="button" class="add" v-on:click="openDialog">add</button>
            <div class="display">
              <div class="block" v-for="(item, index) in arr" :key="index">
                <span>名稱：</span>
                <span>{{ item.name }}</span>
                <span> 金額：$</span>
                <span>{{ item.money }}</span>
                <button type="button" @click="removeTask(index)">dele</button>
              </div>
              <!-- <span>名稱:{{  name  }}金額:{{  money  }}</span> -->
              <!-- <span>{{ item.money }}</span> -->
            </div>
            <button type="button" @click="upDate">upDate</button>
            <Dialog :visible="dialogVisible" @close="closeDialog">
                <span>名稱：</span>
                <input type="text" v-model="newname" placeholder="輸入名稱">
                <span> 金額：$</span>
                <input type="number" v-model="newnumber" placeholder="輸入金額">
              <button v-on:click="addTask" @click="closeDialog" >完成</button>
            </Dialog>
      </div>
    </div>
  </div>
  <!--
<p>{{ title }}</p>
<div class="d">123</div>
v-mode 雙向綁定 
<input type="text" v-model="text">
<p>{{ text }}</p>

v-for foreach 
<p v-for="item in arr">{{ item }}</p>

  v-on
<button type="button" v-on:click="cool">btn</button> 
  -->
</template>

<style lang="scss" scoped>
.area {
  width: 100vw;
  height: 100vh;
  border: 1px solid #000;
  display: flex;
  .left {
    width: 30%;
    height: 100%;
    color: white;
    background-color: #1c587d;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    position: relative;

    .title {
      position: absolute;
      top: 10%;
    }

    .name {
      position: absolute;
      top: 35%;
    }

    .balance {
      position: absolute;
      top: 50%;
    }
  }

  .right {
    width: 70%;
    height: 100%;
    //border: 1px solid #000;

    .top {
      width: 100%;
      height: 30%;
      //border: 1px solid #000;
      display: flex;

      .incom {
        width: 50%;
        height: 100%;
        color: #69a50e;
        //border: 1px solid #000;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
      }

      .expense {
        width: 50%;
        height: 100%;
        color: #c40000;
        //border: 1px solid #000;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
      }
    }

    .bottom {
      width: 100%;
      height: 70%;
      //border: 1px solid #000;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      dialog{
        width: 50%;
        height: 50%;
        border: 1px solid black;
      }
      .add {
        width: 50%;
        height: 10%;
        border-radius: 20px;
        background-color: #1c587d;
        color: white;
        margin-bottom: 50px;
        font-size: 20pt;
      }

      .display {
        width: 70%;
        height: 70%;
        //border: 1px solid #000;
        overflow: auto;
        //overflow: hidden;
        //display: flex;
        //flex-direction: column;
        //justify-content: center;
        //align-items: center;
        .block {
          width:98%;
          height: 15%;
          font-size: 16pt;
          border: 5px solid #1c587d;
          border-radius: 10px;
          margin-bottom: 5px;
          display: flex;
          justify-content: space-around;
          align-items: center;
          button{
            width: 15%;
            height: 50%;
            color:white;
            background-color: #1c587d;
          }
        }
      }
    }
  }
}
</style>
