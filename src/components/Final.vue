<template>
  <div>
    <div class="container">
      <header>
        <h1>{{ personalDetail.name }} {{ personalDetail.last }}</h1>
      </header>
      <main>
        <section>
          <h2>Personal Information</h2>
          <ul>
            <li><strong>Name:</strong> {{ personalDetail.name }} {{ personalDetail.last }}</li>
            <li><strong>Email:</strong> {{ personalDetail.email }}</li>
            <li><strong>Phone:</strong> {{ personalDetail.number }}</li>
            <li><strong>D.O.B:</strong> {{ personalDetail.birth }}</li>
            <li v-if="personalDetail.Cmale==true"><strong>Gender:</strong>Male</li>
            <li v-if="personalDetail.Cfemale==true"><strong>Gender:</strong>Female</li>
            <li><strong>Location:</strong>{{ personalDetail.address1 }},{{ personalDetail.city }},
              {{ personalDetail.code }},{{ personalDetail.country }}
            </li>
          </ul>
          <a v-if="linkedinLinks!==''"><h2>LinkedIn Link: </h2>{{ linkedinLinks }}</a>
          <hr>
        </section>

        <section>
          <h2>Personal Summary</h2>
          <p>{{ summary }}</p>
          <hr>
        </section>
       <div v-if="extraSkill.length>0">
        <section v-if="extraSkill[3]!==''">
          <h2>Objective</h2>
          <div id="educationShow">
            <a>{{extraSkill[3]}}</a>
          </div>
          <hr>
        </section>
       </div>
        <section>
          <h2>Education</h2>
          <div v-for="edu in educationHistorys">

            <ul id="educationShow">
              <li><strong>{{ edu.name }}</strong>, {{ edu.city }}, {{ edu.country }} ({{ edu.graduation }})</li>
              <ul id="qualification">
                <li v-for="qual in edu.qualifications">{{ qual.qualification }}</li>
              </ul>
            </ul>

          </div>
          <div v-if="transferHistorys.length>0" v-for="edu in transferHistorys">
            <ul id="educationShow">
              <li><strong>{{ edu.name }}</strong>, {{ edu.city }}, {{ edu.country }} ({{ transferEnd }})</li>
              <ul id="qualification">
                <li>{{ edu.qualification }}, {{ edu.course }}</li>
              </ul>
              <div v-for="ski in transferSkills">
                <ul id="uniSkills">
                  <li v-for="skii in ski.skills">{{ skii }}</li>
                </ul>
              </div>
            </ul>


          </div>
          <div v-for="edu in mdxHistorys">
            <ul id="educationShow">
              <li><strong>{{ edu.name }}</strong>, {{ edu.city }}, {{ edu.country }} ({{ mdxEnd }})</li>
              <a v-if="transferHistorys.length>0">Transferred from {{ transferHistorys[0].name }} to Middlesex
                University in year {{ parseInt(transferYears) - 3 }} </a>
              <ul id="qualification">
                <li>{{ edu.qualification }}, {{ edu.course }}</li>
              </ul>

              <ul id="uniSkills">
                <li v-for="ski in mdxSkills"><h5>{{ ski.module }} (Grade: {{ sendGrade(ski.Grade) }}) :</h5>
                  {{ ski.skill }}
                </li>
              </ul>

            </ul>

          </div>
          <hr>
        </section>
        <section v-if="workHistory.length>0">
          <h2>Work Experience</h2>
          <div v-for="work in workHistory">
            <ul id="educationShow">
              <li><strong>{{ work.name }}</strong>, {{ work.city }}, {{ work.country }}</li>
              <ul id="workRoles" v-if="work.roles.length>0">
                <li v-for="role in work.roles"><strong>Role: </strong>{{ role.roles }}</br><a
                    v-if="role.descriptions!==''"><strong>Description: </strong></a>{{ role.descriptions }}
                </li>
              </ul>
            </ul>
          </div>
          <hr>
        </section>
        <section v-if="skill.length>0">
          <h2>Skills</h2>
          <ul id="educationShow">
            <li v-for="skills in skill">{{ skills }}</li>
          </ul>
          <hr>
        </section>
        <section v-if="websitesLinks.length>0">
          <h2>Websites</h2>

          <ul id="educationShow">

            <li v-for="web in websitesLinks"><h4>Website Name:</h4> {{ web.name }}</br><h4>Websites Link:</h4> {{ web.link }}</li>

          </ul>

          <hr>
        </section>
        <section v-if="extraSkill.length>0">
          <section v-if="extraSkill[2].length>0">
            <h2>Achievements</h2>
            <div >
            <ul id="educationShow">
            <li v-for="dat in extraSkill[2]"><h4>Achievement:</h4> {{dat.name}}</br><a v-if="dat.description!==''"><h4>Description:</h4> {{dat.description}}</a></li>
            </ul>
            </div>
            <hr>
          </section>

          <section v-if="extraSkill[0].length>0">
            <h2>Community Experience</h2>
            <div>
              <ul id="educationShow">
                <li v-for="dat in extraSkill[0]"><h4>Experience:</h4> {{dat.name}}</br><a v-if="dat.description!==''"><h4>Description:</h4> {{dat.description}}</a></li>
              </ul>
            </div>
            <hr>
          </section>
          <section v-if="extraSkill[4].length>0">
            <h2>Hobbies</h2>
            <ul id="educationShow">
              <li v-for="hob in extraSkill[4]">{{ hob }}</li>
            </ul>
            <hr>
          </section>

          <section v-if="extraSkill[1]!==''">
            <h2>Personal Statement:</h2>
            <div id="educationShow">
              <a>{{extraSkill[1]}}</a>
            </div>
            <hr>
          </section>

        </section>
      </main>

    </div>


  </div>
</template>

<script>
export default {
  name: "Final",
  components: {},

  props: ["personalDetail", "summary", "educationHistorys", "websitesLinks", "transferHistorys", "mdxHistorys", "mdxSkills", "transferSkills", "linkedinLinks", "extraSkill", "transferYears", "skill", "workHistory"],
  data() {
    return {
      transferEnd: '',
      mdxEnd: '',
      show: true,
    }
  },
  created() {

    console.log(this.extraSkill);
    if (this.transferHistorys.length > 0) {
      let temp = this.transferHistorys[0].end.split("-");
      this.transferEnd = temp[0];

    }
    if (this.mdxHistorys.length > 0) {
      let temp = this.mdxHistorys[0].end.split("-");
      this.mdxEnd = temp[0];

    }
    document.body.style.backgroundColor = "#f2f2f2";
  },
  methods: {

    sendGrade(grade) {
      if (parseInt(grade) >= 1 && parseInt(grade) <= 4) {
        return 'First Class';
      }
      if (parseInt(grade) >= 5 && parseInt(grade) <= 8) {
        return 'Upper Second Class';
      }
      if (parseInt(grade) >= 9 && parseInt(grade) <= 12) {
        return 'Lower Second Class';
      }
      if (parseInt(grade) >= 13 && parseInt(grade) <= 16) {
        return 'Third Class';
      }
      if (parseInt(grade) >= 17 && parseInt(grade) <= 20) {
        return 'Fail';
      }
      return '';
    },
  }
}
</script>

<style scoped>

.container {
  max-width: 800px;
  margin: 50px auto;
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

h1, h2 {
  margin: 0;
  padding: 0;
}

h1 {
  font-size: 36px;
  color: #333;
  margin-bottom: 10px;
}

h2 {
  font-size: 24px;
  color: #333;
  margin-top: 20px;
  margin-bottom: 10px;
}

p {
  margin: 0;
  padding: 0;
}

section {
  margin-top: 30px;
}

section:last-child {
  margin-bottom: 0;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

ul li {
  margin-bottom: 5px;
}

ul li:last-child {
  margin-bottom: 0;
}
</style>