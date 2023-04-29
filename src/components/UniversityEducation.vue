<template>
  <div>
  <h2 id="requireText">Education</h2>

  <h3>{{this.miniTitle}}</h3>
  <hr>
  <div class="input-box">
    <label>Institution Name</label>
    <select v-model="institutionName" v-on:change="editSave" value=institutionName>
      <option >Middlesex University</option>
    </select>
    <!--          <input type="text" v-model="institutionName" placeholder="Enter Institution name" required value=institutionName v-on:input="editSave"/>-->
  </div>
  <div class="column">
    <div class="input-box">
      <label>City/Town</label>
      <input type="text" :disabled="disableInput==true" v-model="institutionCity" placeholder="Enter Instituion City" required value=institutionCity />
    </div>
    <div class="input-box">
      <label>Country</label>
      <input type="text" :disabled="disableInput==true" v-model="institutionCountry" placeholder="Enter Instituion City" required value=institutionCountry />
    </div>
  </div>
  <div class="column">
    <div class="input-box">
      <label>Qualification</label>
      <select v-model="qualification" v-on:change="editSave" value=qualification>
        <option >Bachelors</option>
      </select>
    </div>
    <div class="input-box">
      <label>Course Title</label>
      <select v-model="courseTitle" value=courseTitle>
        <option :value="course.name" v-for="course in courseDetails">{{course.name}}</option>
      </select>
    </div>
  </div>
  <div class="column">
    <div class="input-box">
      <label>Start Date</label>
      <input type="date" v-model="startDate" placeholder="mm/dd/yyyy" required value=startDate />
    </div>
    <div class="input-box">
      <label>End Date</label>
      <input type="date" v-model="endDate" placeholder="mm/dd/yyyy" required value=endDate />
    </div>
  </div>
    <div class="gender-box">
      <h3>Transferred In</h3>
      <div class="gender-option">
        <div class="gender">
          <input v-model="checkTransfer" type="checkbox" id="check-male" name="transfer" value=checkTransfer v-on:click="addTransfer"/>
          <a>Yes</a>
        </div>
      </div>
    </div>
    <div v-if="checkTransfer==true">
      <hr>
      <h3>Transferred University Details</h3>
      <div class="input-box">
        <label>Institution Name</label>
                  <input type="text" v-model="transferinstitutionName" placeholder="Enter Institution name" required value=transferinstitutionName v-on:input="editSave"/>
      </div>
      <div class="column">
        <div class="input-box">
          <label>City/Town</label>
          <input type="text"  v-model="transferinstitutionCity" placeholder="Enter Instituion City" required value=transferinstitutionCity />
        </div>
        <div class="input-box">
          <label>Country</label>
          <input type="text"  v-model="transferinstitutionCountry" placeholder="Enter Instituion City" required value=transferinstitutionCountry />
        </div>
      </div>
      <div class="column">
        <div class="input-box">
          <label>Qualification</label>
          <input type="text"  v-model="transferQualification" placeholder="Enter Qualification (Eg. Bachelors)" required value=transferQualification />
        </div>
      </div>
        <div class="input-box">
          <label>Course Title</label>
          <input type="text"  v-model="transferCourse" placeholder="Enter Qualification (Eg. Computer Science)" required value=transferCourse />
        </div>

      <div class="column">
        <div class="input-box">
          <label>End Date</label>
          <input type="date" v-model="transferendDate" placeholder="mm/dd/yyyy" required value=transferendDate />
        </div>

    </div>
    </div>




  <hr>
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
  name: "UniversityEducation",
  props: ['mdxHistorys','transferHistorys'],
  data() {
    return {
      buttonText: 'Next',
      miniTitle: 'University Education',
      feed: '',
      disableInput:false,
      mdxHistory: [],
      transferHistory:[],
      institutionName: '',
      institutionCity: '',
      institutionCountry: '',
      qualification: '',
      endDate: '',
      startDate: '',
      courses:[],
      courseTitle: '',
      courseDetails: [],
      checkTransfer:false,
      transferinstitutionName: '',
      transferinstitutionCity: '',
      transferinstitutionCountry: '',
      transferQualification: '',
      transferendDate: '',
      transferCourse: '',


    }
  },
  created:async function() {
    let corsesResult = await axios.get("https://cvbuildproject-env.eba-pzdzkfze.eu-west-2.elasticbeanstalk.com/courses");
    this.courses = corsesResult.data;
    if(this.mdxHistorys.length!==undefined && this.mdxHistorys.length>0){
      this.mdxHistory=this.mdxHistorys;
      this.institutionName=this.mdxHistory[0].name;
      this.institutionCity=this.mdxHistory[0].city;
      this.institutionCountry=this.mdxHistory[0].country;
      this.qualification=this.mdxHistory[0].qualification;
      this.courseTitle=this.mdxHistory[0].course;
      this.startDate=this.mdxHistory[0].start;
      this.endDate=this.mdxHistory[0].end;
      if(this.institutionName=="Middlesex University"){

        this.courseDetails = this.courses;
      }


    }
    if(this.transferHistorys.length!==undefined && this.transferHistorys.length>0){
      this.transferHistory=this.transferHistorys;
      this.checkTransfer=true;
      this.transferinstitutionName=this.transferHistory[0].name;
      this.transferinstitutionCity=this.transferHistory[0].city;
      this.transferinstitutionCountry=this.transferHistory[0].country;
      this.transferQualification=this.transferHistory[0].qualification;
      this.transferCourse=this.transferHistory[0].course;
      this.transferendDate=this.transferHistory[0].end;
    }
    },
  methods: {


    addTransfer(){
if(this.checkTransfer==true){


}

    },

    editSave: async function () {

      if (this.institutionName == "Middlesex University") {
        this.institutionCity = "London"
        this.institutionCountry = "England";
        this.disableInput = true;
        this.courseDetails = this.courses;
      }
    },
    prevPage:function (){
      this.$emit('changeTitle','Education and Websites link');
      this.$emit('changePage', 'Education');


    },
    nextPage:function (){

      if(this.institutionName!==undefined && this.institutionName!=="" && this.courseTitle!==undefined && this.courseTitle!=="" && this.institutionCountry!==undefined && this.institutionCountry!=="" && this.institutionCity!==undefined && this.institutionCity!=="" && this.qualification!==undefined && this.qualification!=="" && this.endDate!==undefined && this.endDate!=="" && this.startDate!==undefined && this.startDate!==""){
        let temp1=this.startDate.split("-");
        let date=new Date();
        let day=date.getDate();
        let month=date.getMonth();
        let year=date.getFullYear();
        let temp2=this.endDate.split("-");
        let tem1=parseInt(temp1[0]);
        let tem2=parseInt(temp2[0]);
        let time1=new Date(this.startDate).getTime();
        let time2=new Date(this.endDate).getTime();
        let tim1=new Date(time1);
        tim1.setHours(date.getHours() + 365*24);
        let diff=tim1.getHours();
        tim1.setHours(date.getHours() - diff);
        let tim2=new Date(tim1).getTime();
        let todayTime=new Date().getTime();





      if(time1>time2){
          this.feed="Start date is greater than end date"
        }
        else if(tim2>=time2){
          this.feed="Please check the start and end dates";
        }

        else if(todayTime<=time1)
        {
          this.feed="Start date is greater than today's date";

        }

        else {
        if (this.checkTransfer == true) {
          if (this.transferinstitutionName !== undefined && this.transferinstitutionName != "" && this.transferinstitutionCountry !== undefined && this.transferinstitutionCountry != "" && this.transferinstitutionCity !== undefined && this.transferinstitutionCity != "" && this.transferQualification !== undefined && this.transferQualification != "" && this.transferCourse !== undefined && this.transferCourse != "" && this.transferendDate !== undefined && this.transferendDate != "") {
          let tdate=new Date(this.transferendDate).getTime();
          let udate=new Date(this.startDate).getTime();
if(tdate<udate) {
  this.mdxHistory = [];
  this.transferHistory = [];
  let newEducation = {
    name: this.institutionName,
    city: this.institutionCity,
    country: this.institutionCountry,
    qualification: this.qualification,
    course: this.courseTitle,
    start: this.startDate,
    end: this.endDate
  };
  if (this.mdxHistory.length <= 1) {
    this.mdxHistory.push(newEducation);
  }
  newEducation = {};
  newEducation = {
    name: this.transferinstitutionName,
    city: this.transferinstitutionCity,
    country: this.transferinstitutionCountry,
    qualification: this.transferQualification,
    course: this.transferCourse,
    end: this.transferendDate
  };
  this.transferHistory.push(newEducation);

  this.$emit('changeMdx', this.mdxHistory);
  this.$emit('changeTransfer', this.transferHistory);
  this.feed = "";
  this.$emit('changeTitle', 'Education and Websites link');
  this.$emit('changePage', 'Links');
}
else {
  this.feed = "Transfer end date is colliding with Middlesex start date";
}

          } else {
            this.feed = "Please fill all details";
          }
        }
        else{
          this.mdxHistory = [];
        let newEducation = {
          name: this.institutionName,
          city: this.institutionCity,
          country: this.institutionCountry,
          qualification: this.qualification,
          course: this.courseTitle,
          start: this.startDate,
          end: this.endDate
        };
        if (this.mdxHistory.length <= 1) {
          this.mdxHistory.push(newEducation);
        }
        this.$emit('changeMdx', this.mdxHistory);
        this.feed = "";
        this.$emit('changeTitle', 'Education and Websites link');
        this.$emit('changePage', 'Links');

      }
        }

      }
      else{
        this.feed="Please fill all details";
      }



    },

  },
}
</script>

<style scoped>

</style>