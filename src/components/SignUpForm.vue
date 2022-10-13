<template>
 
 <article>
  <h1>Выбор профессии</h1>
</article>

  <form  @submit.prevent="cookieSubmit"> <!-- or @submit="cookieSubmit" without preventDefault-->
   
   <label>Город:</label>
   <select required v-model="city" @change="cityClick($event.target.value)">
     <option value="samara" v-bind:disabled="disableFirstGroup1" >Самара</option>
     <option value="moscow" v-bind:disabled="disableFirstGroup1" >Москва</option>
     <option value="s_peterburg" v-bind:disabled="disableSecondGroup1">Санкт-Петербург</option>
     <option value="orenburg" v-bind:disabled="disableSecondGroup1">Оренбург</option>
     <option value="ufa" v-bind:disabled="disableThirdGroup1">Уфа</option>
     <option value="sochi" v-bind:disabled="disableThirdGroup1">Сочи</option>
   </select>

   <label>Цех:</label>
   <select required v-model="workshop"  @change="workshopClick($event.target.value)"> <!-- v-on:input="workshopClick($event.target.value)"-->
     <option value="main_workshop" v-bind:disabled="disableFirstGroup2" >Основной</option>
     <option value="auxiliary_workshop" v-bind:disabled="disableSecondGroup2" >Вспомогательный</option>
     <option value="service_workshop" v-bind:disabled="disableThirdGroup2" >Обслуживающий</option>
   </select>

   <label>Сотрудник:</label>
   <select required v-model="role" @change="roleClick($event.target.value)">
     <option value="accountant" v-bind:disabled="disableFirstGroup3">Бухгалтер</option>
     <option value="purchasing_manager" v-bind:disabled="disableFirstGroup3">Менеджер по закупкам</option>
     <option value="hr_manager" v-bind:disabled="disableSecondGroup3">Менеджер по кадрам</option>
     <option value="guard" v-bind:disabled="disableThirdGroup3">Охранник</option>
     <option value="cleaner" v-bind:disabled="disableThirdGroup3">Уборщик</option>
     <option value="loader" v-bind:disabled="disableThirdGroup3">Грузчик</option>
   </select>

   <label>Смена:</label>
   <select required v-model="change" @change="changeClick($event.target.value)">
     <option value="change_first" v-bind:disabled="disableFG1">с 8 до 20:00</option>
     <option value="change_second" v-bind:disabled="disableFG2">с 20:00 до 8:00</option>
   </select>

   <label>Бригада:</label>
   <select required v-model="brigade" @change="brigadeClick($event.target.value)">
     <option value="brigade_1" v-bind:disabled="disableFG11">Первая</option>
     <option value="brigade_2" v-bind:disabled="disableFG11">Вторая</option>
     <option value="brigade_3" v-bind:disabled="disableFG11">Третья</option>
     <option value="brigade_4" v-bind:disabled="disableFG22">Четвертая</option>
     <option value="brigade_5" v-bind:disabled="disableFG22">Пятая</option>
   </select>
  

   <div class="terms">
     <input type="checkbox" v-model="terms" required>
     <label>Все данные верны</label>
   </div>


   <div class="submit">
     <button>Подать заявку</button>
    <button type="reset" @click="reloadPage">Сбросить</button>
   </div>

 </form>
  <!-- Проверка получения данных
    <p> City: {{ city}}</p>
 <p> Workshop: {{ workshop}}</p>
 <p> Role: {{ role}}</p>
 <p> Смена: {{ change}}</p>
 <p> Бригада: {{ brigade}}</p>
  -->
 

<footer class="footer">&copy;<span>2022 Designed by Bogdan. <a href="https://github.com/B0gda">GitHub</a></span></footer>

  

</template>

<script>


export default {
  data(){
    return{
      role: '',
      terms: false,
      city:'',
      workshop:'',
      change:'',
      brigade:'',
      disableFirstGroup1:false,
      disableFirstGroup2:false,
      disableFirstGroup3:false,
      disableSecondGroup1:false,
      disableSecondGroup2:false,
      disableSecondGroup3:false,
      disableThirdGroup1:false,
      disableThirdGroup2:false,
      disableThirdGroup3:false,
      disableFG1: false,
      disableFG2: false,
      disableFG11: false,
      disableFG22: false
    }
  },
  methods: {
   
    reloadPage() {
      window.location.reload();
    },
    cookieSubmit(){

          function getCookie(cname) {
          var name = cname + '=';
          var decodedCookie = decodeURIComponent(document.cookie);
           var ca = decodedCookie.split(';');
         for (var i = 0; i < ca.length; i++) {
              var c = ca[i];
              while (c.charAt(0) == ' ') {
                  c = c.substring(1);
              }
              if (c.indexOf(name) == 0) {
                 return c.substring(name.length, c.length);
              }
         }
         return '';
          }

          function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays*24*60*60*1000));
    var expires = 'expires='+ d.toUTCString();
    document.cookie = cname + '=' + cvalue + ';' + expires + ';path=/';
          }
      var data = {
        change: this.change,
        role: this.role,
        workshop: this.workshop,
        brigade:  this.brigade,
        city: this.city,
        };
        console.log(data); //Показ обьекта 
        let resObj = JSON.stringify(data);
        setCookie('user_info', resObj, 7); 
        var savedUserJsonString = getCookie('user_info'); 
        console.log(savedUserJsonString) //проверка выводимых результатов в cookie в формате JSON
    },
    /*Связи между городами-цехами-сотрудниками:
    Для искусственного разделения по "направлениям", 3 группы были разбиты следующим образом:
    1) Первые 2 пукта из города <-> первый  цех <-> первые 2 типа сотрудника
    2) Последующие 2 города <->  второй цех <-> третий тип сотрудника
    2) Последние 2 города <->  третий цех <-> все оставшиеся типы сотрудников
    */
    cityClick(value){
      if(value === 'samara' || value === 'moscow' ){
        this.disableFirstGroup1 = false; this.disableSecondGroup1= false; this.disableThirdGroup1 =  false;
        this.disableFirstGroup2 =  false; this.disableSecondGroup2 = true; this.disableThirdGroup2 = true;
        this.disableFirstGroup3 =  false; this.disableSecondGroup3 = true;  this.disableThirdGroup3 = true;
      } else if (value === 's_peterburg' || value === 'orenburg' ){
        this.disableFirstGroup1 = false; this.disableSecondGroup1= false; this.disableThirdGroup1 =  false;
        this.disableFirstGroup2 =  true; this.disableSecondGroup2 = false; this.disableThirdGroup2 = true;
        this.disableFirstGroup3 =  true; this.disableSecondGroup3 = false;  this.disableThirdGroup3 = true;
      } else {
        this.disableFirstGroup1 = false; this.disableSecondGroup1= false; this.disableThirdGroup1 =  false;
        this.disableFirstGroup2 =  true; this.disableSecondGroup2 = true; this.disableThirdGroup2 = false;
        this.disableFirstGroup3 =  true; this.disableSecondGroup3 = true;  this.disableThirdGroup3 = false;
      }
    },
    workshopClick(value){
      if(value === 'main_workshop' ){
        this.disableFirstGroup1 = false; this.disableSecondGroup1= true; this.disableThirdGroup1 =  true;
        this.disableFirstGroup2 =  false; this.disableSecondGroup2 = false; this.disableThirdGroup2 = false;
        this.disableFirstGroup3 =  false; this.disableSecondGroup3 = true;  this.disableThirdGroup3 = true;
      } else if (value === 'auxiliary_workshop'){
        this.disableFirstGroup1 = true; this.disableSecondGroup1= false; this.disableThirdGroup1 =  true;
        this.disableFirstGroup2 =  false; this.disableSecondGroup2 = false; this.disableThirdGroup2 = false;
        this.disableFirstGroup3 =  true; this.disableSecondGroup3 = false;  this.disableThirdGroup3 = true;
      } else {
        this.disableFirstGroup1 = true; this.disableSecondGroup1= true; this.disableThirdGroup1 =  false;
        this.disableFirstGroup2 =  false;  this.disableSecondGroup2 = false;  this.disableThirdGroup2 = false;
        this.disableFirstGroup3 =  true; this.disableSecondGroup3 = true;  this.disableThirdGroup3 = false;
      }
    },
    roleClick(value){
      if(value === 'accountant' || value === 'purchasing_manager' ){
        this.disableFirstGroup1 = false; this.disableSecondGroup1= true;  this.disableThirdGroup1 =  true;
        this.disableFirstGroup2 =  false; this.disableSecondGroup2 = true; this.disableThirdGroup2 = true;
        this.disableFirstGroup3 =  false; this.disableSecondGroup3 = false; this.disableThirdGroup3 = false;
      } else if (value === 'hr_manager'){
        this.disableFirstGroup1 = true; this.disableSecondGroup1= false; this.disableThirdGroup1 =  true;
        this.disableFirstGroup2 =  true; this.disableSecondGroup2 = false; this.disableThirdGroup2 = true;
        this.disableFirstGroup3 =  false; this.disableSecondGroup3 = false; this.disableThirdGroup3 = false;
      } else {
        this.disableFirstGroup1 = true; this.disableSecondGroup1= true; this.disableThirdGroup1 =  false;
        this.disableFirstGroup2 =  true; this.disableSecondGroup2 = true; this.disableThirdGroup2 = false;
        this.disableFirstGroup3 =  false; this.disableSecondGroup3 = false; this.disableThirdGroup3 = false;
      }
    }, /*Связи между смены-бригады:
    первые три бригады связаны с первой сменой(с 8:00 до 20:00), а 4 и 5 бригады связаны с второй сменой(с 20:00 до 8:00)
     */
    changeClick(value){
      if(value === 'change_first'){
        this.disableFG1 = false; this.disableFG2 = false;
        this.disableFG11 = false; this.disableFG22 = true;
      }else {
        this.disableFG1 = false; this.disableFG2 = false;
        this.disableFG11 = true; this.disableFG22 = false;
      }
    },
    brigadeClick(value) {
      if(value === 'brigade_1' || value === 'brigade_2' || value === 'brigade_3' ){
        this.disableFG1 = false; this.disableFG2 = true;
        this.disableFG11 = false; this.disableFG22 = false;
      }else {
        this.disableFG1 = true; this.disableFG2 = false;
        this.disableFG11 = false; this.disableFG22 = false;
      }
    }
  }
}
</script>

<style>
form{
    max-width: 420px;
    margin: 30px auto;
    background: #FFF;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label{
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input, select{
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
input[type="checkbox"] {
  display: inline-block;
  width: 16px;
  margin: 0 10px 0 0;
  position: relative;
  top: 2px;
}
.pill{
  display: inline-block;
  margin: 20px 10px 0 0;
  padding: 6px 12px;
  background: #eee;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
  cursor: pointer;
}
button {
 background: #0b6dff;
 border: 0;
 padding: 10px 20px;
 margin-top: 20px;
 margin-left: 10px;
 margin-right: 10px;
 color: #FFF;
 border-radius: 20px;
 cursor: pointer;
}
.submit{
  text-align: center;
}
.error{
  color: #ff0062;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}

h1, p, body {
  margin: 0;
}

h1 {
  font-size: 10vmin;
  line-height: 1.1;
  max-inline-size: 15ch;
  margin: auto;
}
a {
  text-decoration: none;
  color:#000
}

.footer {
  position: relative;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 2rem;
  background: rgb(179, 179, 179);
  color: white;
  font-weight: 500;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.2rem;
}

</style>
