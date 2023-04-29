<template>
  <div>
      <div class="column">
        <div class="input-box">
          <label>First Name</label>
          <input type="text" v-model="personalDetails.name" placeholder="Enter First name" required value=personalDetails.name />
        </div>
        <div class="input-box">
          <label>Last Name</label>
          <input v-model="personalDetails.last" type="text" placeholder="Enter Last name" required value=personalDetails.last/>
        </div>
      </div>

      <div class="input-box" >
        <label>Email Address</label>
        <input v-model="personalDetails.email" type="text" placeholder="Enter email address" required value=personalDetails.email/>
      </div>

      <div class="column"  >
        <div class="input-box">
          <label>Phone Number</label>
          <input v-model="personalDetails.number" type="number" placeholder="Enter phone number" required value=personalDetails.number/>
        </div>
        <div class="input-box">
          <label>Birth Date</label>
          <input v-model="personalDetails.birth" type="date" placeholder="Enter birth date" required v-on:input="checkDate" value=personalDetails.birth/>
        </div>
      </div>
      <div class="gender-box" >
        <h3>Gender</h3>
        <div class="gender-option">
          <div class="gender">
            <input v-model="personalDetails.Cmale" type="checkbox" id="check-male" name="gender" value=personalDetails.Cmale/>
            <a>male</a>
          </div>
          <div class="gender">
            <input v-model="personalDetails.Cfemale" type="checkbox" id="check-female" name="gender" value=personalDetails.Cfemale />
            <a>Female</a>
          </div>
          <div class="gender">
            <input v-model="personalDetails.CNmale" type="checkbox" id="check-other" name="gender" value=personalDetails.CNmale />
            <a>prefer not to say</a>
          </div>
        </div>
      </div>
      <div class="input-box address">
        <label>Address</label>
        <input v-model="personalDetails.address1" type="text" placeholder="Enter street address" required value=personalDetails.address1/>
        <input v-model="personalDetails.address2" type="text" placeholder="Enter street address line 2" required value=personalDetails.address2/>
        <div class="column">
          <div  class="select-box">
            <select v-model="personalDetails.country" value=personalDetails.country  v-on:change="changeSelect">
              <option :value="personalDetails.country" hidden>{{this.scountry}}</option>
              <option :value="item.name.common" v-for="item in countryoption">{{ item.name.common }}
              </option>
            </select>
          </div>
          <div  class="select-box">
            <select v-model="personalDetails.nationality" value=personalDetails.nationality v-on:change="changeSelect">
              <option :value="personalDetails.nationality" hidden>{{this.snationality}}</option>
              <option :value="items" v-for="items in nationalityoption">{{ items}}
              </option>
            </select>
          </div>
          <input v-model="personalDetails.city" type="text" placeholder="Enter your city" required value=personalDetails.city/>
        </div>
        <div class="column">
          <input v-model="personalDetails.region" type="text" placeholder="Enter your region" required value=personalDetails.region/>
          <input v-model="personalDetails.code" type="text" placeholder="Enter postal code" required value=personalDetails.code/>
        </div>
      </div>
      <div class="column">
        <a id="feedback" v-html="feed"></a>
      </div>
      <div class="column">
        <button id="formButton" v-on:click="nextPage">{{this.buttonText}} →</button>
      </div>
  </div>

</template>

<script>
export default {
  name: "PersonalDetails",
  props:["personalDetail"],
  data() {
    return {
      countryoption:[],
      nationalityoption:[],
      buttonText:'Next',
      scountry:"Country",
      personalDetails:{name:'',last:'',email:'',number:0,address1:'',address2:'',country:'Country',nationality:'Nationality',city:'',region:'',code:'',birth:'',Cmale:false,Cfemale:false,CNmale:false},
      snationality:"Nationality",
      feed:'',


    }
  },
  components: {},
  created: function () {
    let app=this;
    let nationalHtml = "";
    fetch("https://restcountries.com/v3.1/all")
        .then(function (response) {
          response.json().then(
              function (json) {
                app.countryoption = json.sort(function (a, b) {
                  if (b.name.common > a.name.common)
                    return -1;
                  if (b.name.common < a.name.common)
                    return 1;
                  return 0;
                });
              });

        });

    fetch("https://restcountries.com/v3.1/all")
        .then(function (response) {
          response.json().then(
              function (json) {
                let temp = json.sort(function (a, b) {
                  try {
                    if (b.demonyms.eng.f > a.demonyms.eng.f)
                      return -1;
                    if (b.demonyms.eng.f < a.demonyms.eng.f)
                      return 1;
                    return 0;
                  } catch {
                  }
                });

                temp.forEach(nat => {
                  try {
                    app.nationalityoption.push(nat.demonyms.eng.f)
                  } catch {
                  }
                })


              });


        });

    if(this.personalDetail.name!==""){
      this.personalDetails=this.personalDetail;
    }
  },
  methods:{
    checkDate:function (){
      const date = new Date();
      let day = date.getDate();
      let month = date.getMonth() + 1;
      let year = date.getFullYear();
      let check=this.personalDetails.birth.split("-");
      console.log(year)
      console.log(check[2])
      if((year-15)<=parseInt(check[0])){
        this.feed="Date error";
        this.personalDetails.birth="";
      }
      else{
        this.feed="";
      }

    },
    changeSelect:function (){
      if(this.personalDetails.country!==undefined){
        this.scountry=this.personalDetails.country;
      }
      if(this.personalDetails.nationality!==undefined) {
        this.snationality = this.personalDetails.nationality;
      }

    },
    nextPage: function () {

        if (this.personalDetails.name!== "" && this.personalDetails.last!== "" && this.personalDetails.email!=="" && this.personalDetails.number!==0 && this.personalDetails.address1!=="" && this.personalDetails.address2!=="" && this.personalDetails.country!=="Country" && this.personalDetails.nationality!=="Nationality" && this.personalDetails.city!=="" && this.personalDetails.region!=="" && this.personalDetails.code!=="" && this.personalDetails.birth!=="" &&
            this.personalDetails.name!== undefined  && this.personalDetails.last!== undefined  && this.personalDetails.email!== undefined  && this.personalDetails.number!== undefined  && this.personalDetails.address1!== undefined  && this.personalDetails.address2!== undefined  && this.personalDetails.country!== undefined  && this.personalDetails.nationality!== undefined && this.personalDetails.city!== undefined && this.personalDetails.region!== undefined && this.personalDetails.code!== undefined && this.personalDetails.birth !== undefined){
          this.feed="";
          this.$emit('changePersonal',this.personalDetails);
          this.$emit('changeTitle','Personal Summary');
          this.$emit('changePage','Summary');

        }
        else{
          this.feed="Please fill in all the required fields";
        }
      },




    },
}
</script>

<style scoped>

</style>