<!--  -->
<template>
  <div>
    <h2>任务列表</h2>
    <p>总数:{{count}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;已完成:{{num}}</p>
    <p>
      <input type="text"
       v-model="text" 
       v-on:keydown="add" 
       />&nbsp;&nbsp;&nbsp;&nbsp;
      <button class="btn"
      @click="search"
      >搜索</button>
    </p>
    <p>
      <input type="checkbox"
      v-model="finishflag"
      @change="finish" /> 已完成&nbsp;&nbsp;&nbsp;&nbsp;
      <input type="checkbox"
      v-model="unfinishflag"
      @change="unfinish"
       /> 未完成
    </p>
    <ol>
      <li v-for="item in list" :key="item.id">
        <input type="checkbox" v-model="item.check"
        @change='changeone'>
        
        {{item.tit}}
        <button @click="del(item.id)">删除</button>&nbsp;
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <button :class="item.flag?'finish':'unfinish'"
        @click="changeFinish(item)"
        >
          {{item.flag?'已完成':'未完成'}}
          </button>
        </li>
    </ol>
    <p><input type="checkbox"
    v-model="checkAll"
    @change="changeAll">全选</p>
  </div>
</template>

<script>
export default {
  computed: {
    count(){
      return this.list.length
    },
    num(){
      return this.list.filter(item=>item.flag==true).length
    }
  },
  created () {
        this.copylist=[...this.list]

  },
  data() {
    return {
      text: "",
      finishflag:false,
        unfinishflag:false,
        checkAll:false,
      list: [
        {
          id: 0,
          tit: "最终我们都活成了大多数人的样子,随着熙熙攘攘的人流,日复一日",
          flag: true,
          changeflag: false,
          check:false,
        },
         {
          id: 1,
          tit: "你是千堆雪我是长街,怕日出一到彼此瓦解.",
          flag: false,
          changeflag: false,
          check:false,
        }
      ],
      copylist:[]
    };
  },
  methods: {
      add(e) {
    if (e.keyCode === 13) {
      let obj = {
        id: new Date()*1,
        tit: this.text,
        flag: false,
        changeflag: false,
        check:false
      };
      // window.console.log(obj);
      this.list.push(obj)
      this.text=''
    }
  },
  del(id){
    let index=this.list.findIndex(item=>item.id==id)
    this.list.splice(index,1)
  },
  //已完成 未完成改变
  changeFinish(id){
      id.flag=!id.flag
  },
  //搜索
  search(){
    this.list=this.copylist.filter(item=>item.tit.includes((this.text)))
  },
  //全选
  changeAll(){
     this.list.forEach(item=>item.check=this.checkAll)
  },
  //单选
  changeone(){
     this.checkAll =this.list.every(item=>item.check) 
  },
  //已完成
  finish(e){
    this.finishflag=e.target.checked
    // window.console.log(this.finishflag);
    
    if(this.finishflag){
      //  window.console.log(this.copylist,this.list);
         this.list=this.copylist.filter(item=>item.flag==true)

    }  else{
       this.list=this.copylist
    }
},
  //未完成
  unfinish(e){
    // if(this.unfinishflag===this.finishflag){
    //   this.copylist=[...this.list]
    // }
       this.unfinishflag=e.target.checked
    if(this.unfinishflag){
         this.list=this.copylist.filter(item=>item.flag==false)
    }  else{
       this.list=this.copylist
    }
  }
  }
};
</script>
<style>
h2,
p {
  margin-left: 40px;
}
.btn {
  background: #facdcd;
  border: none;
  outline: none;
  border-radius: 3px 3px 3px 3px;
  color: brown;
}
.unfinish {
  background: #f85959;
  border: none;
  outline: none;
  border-radius: 3px 3px 3px 3px;
  color: rgb(88, 31, 31);
}
.finish{
  background: #62f87b;
  border: none;
  outline: none;
  border-radius: 3px 3px 3px 3px;
  color: rgb(32, 75, 29);
}
span{
  margin-left: 20px;
}
</style>