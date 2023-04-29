<template>
  <div>
      <h2 id="requireText">Education</h2>

      <h3>{{this.miniTitle}}</h3>
      <a id="feedbacks">(Please don't include Middlesex University in this section)</a>
      <hr>
      <div  v-for="edu in educationHistory" v-if="showEdu==true">
        <div id="buttonAndH">
          <h4>{{edu.count+1}}. Institution Name: {{edu.name}}</h4> <button v-on:click='deleteInfo(edu.count)' ><img src="/delete.png" width="20" height="20"></button>
        </div>
        <h5>Institution City: {{edu.city}}</h5></br>
        <h5>Institution Country: {{edu.country}}</h5></br>
        <h5>Qualification:</h5>
        <div v-for="qual in edu.qualifications">
          <h5>{{qual.count+1}}. {{qual.qualification}}</h5></br>
        </div>
        <h5>Graduation: {{edu.graduation}}</h5></br>
        <button id='infoButton'  v-on:click='editInfo(edu.count)' v-if="edu.edit==false">Edit</button>
        <button id='infoButton'  v-on:click='undoEdit(edu.count)' v-else>Undo Edit</button>

        <hr>
      </div>
      <div>
        <div id="feedback" v-html="feedB"></div>
      </div>

      <div class="input-box">
        <label>Institution Name</label>
        <input type="text" v-model="institutionName" placeholder="Enter Institution name" required value=institutionName v-on:input="editSave"/>
      </div>
      <div class="column">
        <div class="input-box">
          <label>City/Town</label>
          <input type="text" v-model="institutionCity" placeholder="Enter Instituion City" required value=institutionCity />
        </div>
        <div class="input-box">
          <label>Country</label>
          <input type="text" v-model="institutionCountry" placeholder="Enter Instituion City" required value=institutionCountry />
        </div>
      </div>
      <div class="input-box">
        <label>Qualification</label>
        <div class="column">
        <input type="text" v-model="qualification" placeholder="Enter qualification(e.g. A-Levels, Baachelors)" required value=qualification /><button v-on:click='addQualification' ><img src="/add.png" width="20" height="30"></button>
        </div>
        <div v-for="qualify in qualifications">
          <div class="column">
          <input type="text"  :disabled="true" :value="qualify.qualification" /><button v-on:click='delteQualification(qualify.count)' ><img src="/delete.png" width="20" height="30"></button>
        </div>
        </div>
      </div>
      <div class="input-box">
        <label>Graduation Date</label>
        <input type="date" v-model="endDate" placeholder="mm/dd/yyyy" required value=endDate />
      </div>
      <hr>




      <div class="column">
        <button id="moreButton" v-on:click="saveEdit"  v-if="editData==true">Save</button>
        <button id="moreButton" v-on:click="addEducation" v-if="editData==false">Add</button>
      </div>

    <div class="column">
      <a id="feedback" v-html="feed"></a>
    </div>
    <div class="column">
      <button id="formButton" v-on:click="prevPage">← Previous</button>
      <button id="formButton" v-on:click="nextPage">{{this.buttonText}} →</button>
    </div>



  </div>

</template>

<script>
export default {
  name: "Education",
  props:['educationHistorys'],
  data() {
    return {
      buttonText:'Next',
      miniTitle:'School,High School and other',
      showEdu:false,
      editData:false,
      educationHistory:[],
      qualifications:[],
      institutionName:'',
      institutionCity:'',
      institutionCountry:'',
      qualification:'',
      endDate:'',
      startDate:'',
      courseDetails:[],
      uniHistory:[],
      courseTitle:'',
      disableInput:'false',
      feed:'',
      feedB:'',

    }
  },
  created() {
  if(this.educationHistorys.length!==undefined && this.educationHistorys.length>0){
    this.showEdu=true;
    this.educationHistory=this.educationHistorys;
  }
    },
  methods:{
    addQualification(){

      if(this.qualification!==undefined && this.qualification!==""){

        if(this.qualifications.length<1){
          let newObj={qualification:this.qualification,count:0};
          this.qualifications.push(newObj);
          this.qualification="";
        }
        else{
          let cQualifications=this.qualifications.length-1;
          let newObj={qualification:this.qualification,count:this.qualifications[cQualifications].count+1};
          this.qualifications.push(newObj);
          this.qualification="";
        }
         this.feed="";

      }
      else{
        this.feed="Please enter a qualification to add more";
      }

    },
    delteQualification(param){
      this.qualifications=this.qualifications.filter(web=>web.count!==param);
      this.qualifications.forEach((edu,index)=>{
        edu.count=index;
      });

    },
    editInfo:function (param){
      this.institutionName=this.educationHistory[param].name;
      this.institutionCity=this.educationHistory[param].city;
      this.institutionCountry=this.educationHistory[param].country;
      this.qualifications=this.educationHistory[param].qualifications;
      this.endDate=this.educationHistory[param].graduation;
      this.feedB="Please edit the data below";
      this.educationHistory[param].edit=true;
      this.editData=true;
      this.educationHistory.forEach((edu,index)=>{
        if(index!==param && edu.edit==true){
          edu.edit=false;
        }
      })
      this.editCount=param;

    },
    undoEdit:function (param){
      this.feedB="";
      this.institutionName="";
      this.institutionCity="";
      this.institutionCountry="";
      this.qualification="";
      this.qualifications=[];
      this.endDate="";
      this.educationHistory[param].edit=false;
      this.editData=false;

    },
    editSave:async function (){

        if(this.institutionName.toUpperCase()=="MIDDLESEX UNIVERSITY"){
          this.feedB="Middlesex University can only be added in next section";
          this.institutionCity="";
          this.institutionCountry="";
          this.qualification="";
          this.qualifications=[];
          this.endDate="";
          this.institutionName="";
        }
        else{
          this.feedB="";
        }



    },
    saveEdit:function (){
      if(this.qualification!==undefined && this.qualification!==""){
        let checkDuplicate=false;
        this.qualifications.forEach(qual=>{
          if(qual.qualification==this.qualification){
            checkDuplicate=true;
          }
        });
        if(checkDuplicate==false){
          this.addQualification();
        }

      }
      if(this.institutionName!==undefined && this.institutionName!=="" && this.institutionCountry!==undefined && this.institutionCountry!=="" && this.institutionCity!==undefined && this.institutionCity!=="" && this.qualifications.length>0 && this.endDate!==undefined && this.endDate!=="") {
        this.feed = "";
        this.educationHistory[this.editCount].name=this.institutionName;
        this.educationHistory[this.editCount].city= this.institutionCity;
        this.educationHistory[this.editCount].country=this.institutionCountry;
        this.educationHistory[this.editCount].qualifications= this.qualifications;
        this.educationHistory[this.editCount].graduation= this.endDate;
        this.institutionName="";
        this.institutionCity="";
        this.institutionCountry="";
        this.qualification="";
        this.qualifications=[];
        this.endDate="";
        this.editData=false;
        this.showEdu=true;
        this.educationHistory[this.editCount].edit=false;
        this.editCount=null;
        this.feedB="";

      }
      else{
        this.feed="Fields can't be empty";
      }

    },
    deleteInfo:function (param){
      this.educationHistory=this.educationHistory.filter(web=>web.count!==param);
      this.educationHistory.forEach((edu,index)=>{
        edu.count=index;
      });
    },
    addEducation:function (){
      if(this.qualification!==undefined && this.qualification!==""){
        let checkDuplicate=false;
        this.qualifications.forEach(qual=>{
          if(qual.qualification==this.qualification){
            checkDuplicate=true;
          }
        });
        if(checkDuplicate==false){
          this.addQualification();
        }

      }
      if(this.institutionName!==undefined && this.institutionName!=="" && this.institutionCountry!==undefined && this.institutionCountry!=="" && this.institutionCity!==undefined && this.institutionCity!=="" && this.qualifications.length>0 && this.endDate!==undefined && this.endDate!==""){
        this.feed="";
        let tempDate=this.endDate.split("-");
        if(this.educationHistory.length<1){
          let newEducation={name:this.institutionName,city:this.institutionCity,country:this.institutionCountry,qualifications:this.qualifications,graduation:tempDate[0],count:0,edit:false};
          this.educationHistory.push(newEducation);
        }
        else{
          let tempC=this.educationHistory.length-1;
          let newEducation={name:this.institutionName,city:this.institutionCity,country:this.institutionCountry,qualifications:this.qualifications,graduation:tempDate[0],count:this.educationHistory[tempC].count+1,edit:false};
          this.educationHistory.push(newEducation);
        }
        this.institutionName="";
        this.institutionCity="";
        this.institutionCountry="";
        this.qualification="";
        this.qualifications=[];
        this.endDate="";
        this.showEdu=true;

      }
      else{
        this.feed="Please fill the above section to add more";
      }
    },
    prevPage:function (){
      this.title="Education and Websites link";
      this.miniTitle="University Education"
      this.$emit('changeEducation',this.educationHistory);
      this.$emit('changeTitle','Personal Summary');
      this.$emit('changePage', 'Summary');


    },
    nextPage:function (){
      if(this.educationHistory.length>=1){
        this.feed="";
       // this.miniTitle="University Education"
          this.institutionName="";
          this.institutionCity="";
          this.institutionCountry="";
          this.qualification="";
          this.endDate="";
        this.showEdu=false;
        this.editData=false;
        this.dataEdit=false;
        this.$emit('changeEducation',this.educationHistory);
        this.$emit('changeTitle','Education and Websites link');
        this.$emit('changePage', 'UniversityEducation');
      }
      else{
        this.feed="Please fill atleast one school or high school education history";
      }
    },


  },
}
</script>


