<template>
  <div>
    <h2> App.vue문서</h2>
    <img src="./assets/images/main_4.jpg" height="150" width="300">
    <p></p>
    <input type="text" ref="todoText" size=15 @keyup.enter="insertTodo">
    <!--원본정답 <input type="text" v-model="todoText" size=15 @keyup.enter="insertTodo">-->
    <button @click="insertTodo">todo 등록</button>
    
    
    <div v-for="(item) in todoList" :key="item" style="color:orangered">
      {{ item.checked }} {{ item.id }} - {{ item.text }} &nbsp; 

    </div>

    <hr color="blue" size="3"/>
    <!--App문서에서 구현은 했지만 deleteTodo(id) greenCheckbox({id,checked})-->
    <TodoTest v-for ="(item) in todoList" :key="item.id"
              :skytodo = "item" @click-delete="deleteTodo" @toggle-checkbox="greenCheckbox">

    </TodoTest>
    <p></p>
    <Hello msg="모란백합 Welcome"/>
   
 </div>
</template>

<script>
//import Hello from './components/HelloWorld.vue'
// import BoardTest from './components/BoardTest.vue'
import TodoTest from './components/TodoTest.vue';

export default {
  name: 'App',
 // components: { Hello , BoardTest}
  components:{TodoTest},
  
  data(){
  return {
    kind : '라떼녹차cake' ,
    todoText : 'abc'  ,
    count : 1 ,
    todoList : [
        { id:1, text:"kim", checked:false},
        { id:2, text:"lee", checked:true},
    ]
  }
 },
 methods:{
  insertTodo(){
    // alert("insertTodo 메소드");

    var max = this.todoList.reduce(function(a,b){
      console.log(a);
      return a > b ? a: b.id;
    });

    this.todoList.push({
      id:max+1,
      //id:this.todoList.length+1,
      //text:this.todoText,
      text : this.$refs.todoText.value,
      checked:false
    }); //배열에 넣기
    //this.todoText = '';
    this.$refs.todoText.value='';
    localStorage.setItem("young",JSON.stringify(this.todoList));
    
  },
  deleteTodo(id){ //웹브라우저 storage연결
     var max = this.todoList.reduce(function (a,b) {
      
      return a > b.id ? a : b.id; 

     });

     id=max;
     var index = this.todoList.findIndex(mytodo=>{return mytodo.id==id;});
     this.todoList.splice(index,1);
     localStorage.removeItem(index);
    
    //var max = this.todoList.reduce(function(a,b){
    //  console.log(a);
    //  return a > b ? a: b.id;
    //});
    //배열추가 push(), 대표값하나추출 reduce(), 찾아서 findIndex()
    // todoList배열 데이터추가는 App.vue처리하고, 삭제는 새로만든 컴포넌트 TodoTest.vue처리
    //id = max;
    //var index = this.todoList.findIndex(); // 7월6일 토요일 삭제처리 여기서 부터 시작
    //this.todoList.splice(index,1);
  },
  greenCheckbox({id, checked}){
    //console.log("greenCheckbox() 함수");
    var index = this.todoList.findIndex(mytodo=>{return mytodo.id==id;});
    this.todoList[index].checked = checked;
  },
 }
 
}
</script>

<style>
#app {
  margin-top: 20px;
}
</style>
