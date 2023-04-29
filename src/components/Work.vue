<template>
  <div>
    <a id="feedbacks">(This is an optional section where you have to add to display the work history)</a>
    <hr>
    <h2 id="NotrequireText">Work History</h2>
    <hr v-if="showEdu==true">
    <div  v-for="work in workHistorys" v-if="showEdu==true">
      <div id="buttonAndH">
        <h4>{{work.count+1}}. Company Name: {{work.name}}</h4> <button v-on:click='deleteWork(work.count)' ><img src="/delete.png" width="20" height="20"></button>
      </div>
      <h5>Company City: {{work.city}}</h5></br>
      <h5>Company Country: {{work.country}}</h5></br>
      <h5>Roles:</h5>
      <div v-for="rol in work.roles">
        <div class="rolesBorder">
          <div  class="roleDet">
        <h5> Role: {{rol.roles}}</h5></br>
        <h5>Role Description:</h5><a> {{rol.descriptions}}</a></br>
        </div>

        </div>
        </br>
      </div>

      <hr>
    </div>
    <hr>
    <div class="input-box">
      <a>Company Name</a>
      <input type="text" v-model="companyName" placeholder="Enter Institution name" required value=companyName/>
    </div>
    <div class="column">
      <div class="input-box">
        <a>City/Town</a>
        <input type="text" v-model="companyCity" placeholder="Enter Instituion City" required value=companyCity />
      </div>
      <div class="input-box">
        <a>Country</a>
        <input type="text" v-model="companyCountry" placeholder="Enter Instituion City" required value=companyCountry />
      </div>
    </div>


        <div class="input-box">
      <a>Role</a>
          <div class="column">
      <input type="text" v-model="role" placeholder="Enter Role(e.g. Manager, Team Memeber)" required value=role v-on:input="rolesInput"/><button v-on:click='addRoles' v-if="roleButton==true"><img src="/add.png" width="20" height="30"></button>
          </div>
          <div  v-if="showDes==true">
            <div class="input-box">
              <a>Description</a>
              <ul v-if="descriptionManual==false" class="description-list" >
                <li v-on:click="selectDescription('manual')">+ Add Manually</li>
                <li  v-on:click="selectDescription('auto')">{{this.description}}</li>
              </ul>
              <div v-if="descriptionManual==true">
                <div class="column">
                  <textarea type="text" v-model="description" placeholder="Enter role Description" required value=description /><button id="desButton" v-on:click='addDescription' width="20px" height="30px"><img src="/add.png" width="20" height="30"></button>
              </div>
              </div>
              </div>
          </div>

       <div v-if="roleFound==true">
         <ul class="roles-list" >
           <li v-for="rol in resultRoles" v-on:click="selectRoles(rol)">{{rol}}</li>
         </ul>
       </div>

          <div v-for="rol in roles">
            <div class="rolesBorder">
            <div class="column">
              <input type="text"  :disabled="true" :value="rol.roles" />
            </div>
            <div class="column">
              <input type="text"  :disabled="true" :value="rol.descriptions" /><button v-on:click='deleteRoles(rol)' ><img src="/delete.png" width="20" height="30"></button>
            </div>
            </div>
          </div>
        </div>



<!--    <div class="input-box">-->
<!--      <label>Graduation Date</label>-->
<!--      <input type="date" v-model="endDate" placeholder="mm/dd/yyyy" required value=endDate />-->
<!--    </div>-->
    <hr>
    <button id="moreButton" v-on:click="addWork">Add</button>
    <div class="column">
      <button id="formButton" v-on:click="prevPage">← Previous</button>
      <button id="formButton" v-on:click="nextPage">{{this.buttonText}} →</button>
    </div>

  </div>
</template>

<script>
export default {
  name: "Work",
  props:['skills','jobQualifications','workHistory'],
  data() {
    return {
      companyName:'',
      showDes:false,
      showEdu:false,
      roleFound:false,
      companyCity:'',
      companyCountry:'',
      roles:[],
      roleButton:false,
      resultRoles:[],
      role:'',
      descriptions:[],
      description:'',
      workHistorys:[],
      descriptionManual:false,
      buttonText:'Next',


    }
    },
  created() {
    if(this.workHistory.length>0){
      this.workHistorys=this.workHistory;
      this.showEdu=true;
    }

  },
  methods:{
    deleteWork(param){
      this.workHistorys=this.workHistorys.filter(web=>web.count!==param);
      this.workHistorys.forEach((edu,index)=>{
        edu.count=index;
      });

      if(this.workHistorys.length<1){
        this.showEdu=false;
      }
      },

    addWork(){
      if(this.description==undefined){
        this.description=" ";
      }
      if(this.role==undefined){
        this.role=" ";
      }
      if(this.role!==""){
      this.roles.push({"roles":this.role,"descriptions":this.description});
      this.role='';
      this.description='';
      this.showDes=false;
      this.descriptionManual=false;
      }

      if(this.workHistorys.length==0){
               this.workHistorys.push({"name":this.companyName,"city":this.companyCity,"country":this.companyCountry,"roles":this.roles,"count":0});
               this.showEdu=true;
               this.roles=[];
               this.companyName="";
               this.companyCity="";
               this.companyCountry="";
             }
             else{
               let length=this.workHistorys.length-1;
             this.workHistorys.push({"name":this.companyName,"city":this.companyCity,"country":this.companyCountry,"roles":this.roles,"count":this.workHistorys[length].count+1});
              this.showEdu=true;
             this.roles=[];
             this.companyName="";
             this.companyCity="";
             this.companyCountry="";
             }
    },

    addRoles(){
        let found=false;
        this.roles.forEach(rol=>{
          if(rol.roles==this.role){
            found=true;
          }
        });
        if(found==false){
          this.showDes=true;
          this.descriptionManual=true;
        }


    },
    addDescription(){
      if(this.description==undefined){
        this.description=" ";
      }
      this.roles.push({"roles":this.role,"descriptions":this.description});
      this.role='';
      this.description='';
      this.showDes=false;
      this.descriptionManual=false;

    },
    selectDescription(select){
            if(select=='auto'){
              // this.roles.push({"roles":this.role,"descriptions":this.description});
              // this.role='';
              // this.description='';
              // this.showDes=false;
              // this.descriptionManual=false;

              this.showDes=true;
              this.descriptionManual=true;



            }
            else {
                 this.description='';
                 this.descriptionManual=true;

            }

    },
    deleteRoles(rol){
      this.roles=this.roles.filter(rols=>rols!==rol);


    },
    selectRoles(rol){
      this.role="";
      this.roleFound=false;
      let found=false;
      this.roles.forEach(rols=> {
       if(rols==rol){
         found=true;
       }

      });
      if(found==false){
        this.jobQualifications.forEach(job =>{
          if(job.title==rol){
            this.description=job.description;
          }
        });
        this.role=rol;
        this.showDes=true;
      }

    },
    rolesInput(){
      this.resultRoles=[];
      this.jobQualifications.forEach(job =>{
        this.resultRoles.push(job.title);
      });
      if(this.role!==undefined && this.role!==""){
        this.resultRoles=this.resultRoles.filter((rol) => rol.toLowerCase().includes(this.role.toLowerCase()));
        if(this.resultRoles.length>0){
          this.roleFound=true;
          this.roleButton=false;
        }
        else{
        this.roleButton=true
        }

      }
      else{
        this.resultRoles=[];
        this.roleFound=false;
        this.roleButton=false;
        this.showDes=false;
      }

    },
    prevPage:function (){
      this.$emit('changeWorkHistory',this.workHistorys);
      this.$emit('changeTitle','Education and Websites link');
      this.$emit('changePage', 'Links');


    },
    nextPage:function (){

        this.$emit('changeWorkHistory',this.workHistorys);
        this.$emit('changeTitle','Skills');
        this.$emit('changePage', 'Skill');

    },

  },

}
</script>

