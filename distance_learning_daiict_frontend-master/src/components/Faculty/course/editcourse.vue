<template>

  <v-container fluid="true">
    <navMenu></navMenu>
    <br>
    <v-layout>
      <v-flex>
        <h2 class="text-md-center mb-4 red--text">
          Edit Course
        </h2>
      </v-flex>

    </v-layout>
    <div
      style="min-width: 560px; margin: auto;"
      class="lighten-4">
      <v-card class="grey lighten-4">
      <v-layout row>
        <v-flex xs12 sm6 offset-sm3>
          <v-container>
            <v-form @submit.prevent="" enctype="multipart/form-data">
              <v-text-field
                label="Course Name"
                v-model="it.facultyCourse_name"
                required></v-text-field>
              <v-text-field
                label="Description"
                v-model="it.facultyCourse_description"
                required></v-text-field>
              <label>Course Image*</label><br>
              <input
                type="file"
                @change="onFileSelected2"
                required
                id="facultyCourse_image"
              ></input>
              <v-text-field
                label="Course Duration"
                v-model="it.facultyCourse_duration"
                required
              ></v-text-field>
              <v-text-field
                label="Course Prerequisites"
                v-model="it.facultyCourse_prerequisites"
                required
              ></v-text-field>
              <v-select
                :items="faculty_ta_require"
                label="TA"
                data-vv-name="select"
                required
              ></v-select>


              <v-card-title>
                <h1>Enter Sub Topic Detail</h1>
              </v-card-title>
              <v-text-field
                label="Name"
                v-model="subtopic_name"
                name="subtopic_name"
                required
              ></v-text-field>
              <v-text-field
                label="Description"
                name="subtopic_description"
                v-model="subtopic_description"
                required></v-text-field>
              <label>Upload Video*</label><br>
              <input
                type="file"
                @change="onFileSelected1"
                required
                name="subtopic_video"
                id="subtopic_video"
              ></input><br><br>
              <label>Upload Assignment*</label>
              <input
                type="file"
                @change="onFileSelected"
                required
                name="subtopic_assignment"
              ></input><br><br>
              <v-text-field
                label="Weightage"
                name="subtopic_weightage"
                v-model="subtopic_weightage"
                required></v-text-field>
              <v-text-field
                label="Assignment TotalMarks"
                name="subtopic_assignment_totalMarks"
                v-model="subtopic_assignment_totalMarks"
                required></v-text-field>
              <v-card-actions >
                <v-btn color="purple" class="text--white"  @click="save(subtopic_name, subtopic_description, subtopic_video, subtopic_assignment, subtopic_assignment_totalMarks, subtopic_weightage)"> Add more subtopic </v-btn>
              </v-card-actions>

            </v-form>
          </v-container>
        </v-flex>
      </v-layout>
      </v-card>
    </div>
    <v-data-table
      :headers="headers"
      :items="facultyCourse_subtopics"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td>{{ props.item.subtopic_name }}</td>
        <td class="text-xs-right">{{ props.item.subtopic_description }}</td>
        <td class="text-xs-right">{{ props.item.subtopic_video }}</td>
        <td class="text-xs-right">{{ props.item.subtopic_assignment }}</td>
        <td class="text-xs-right">{{ props.item.subtopic_weightage }}</td>
        <td class="text-xs-right">{{ props.item.subtopic_assignment_totalMarks }}</td>
        <v-btn color="purple" class="text--white iteDelete"
               @click="del(props.item.subtopic_name, props.item.subtopic_description, props.item.subtopic_video.name, props.item.subtopic_assignment.name, props.item.subtopic_weightage, props.item.subtopic_assignment_totalMarks)">
          delete
        </v-btn>
      </template>
      <template slot="pageText" slot-scope="props">
        Lignes {{ props.pageStart }} - {{ props.pageStop }} de {{ props.itemsLength }}
      </template>
    </v-data-table>
    <v-btn color="purple" class="text--white" type="submit" @click="updateCourse"> submit</v-btn>
  </v-container>

</template>
<script>
  import Menu from '@/components/Faculty/menu'
  import axios from 'axios'
  import Vue from 'vue'

  export default {
    components: {'navMenu': Menu},
    data () {
      return {
        sideNav: false,
        course_id: '',
        faculty_id: '',
        facultyCourse_subtopics: [],
        facultyCourse_name: '',
        facultyCourse_image: '',
        facultyCourse_prerequisites: '',
        facultyCourse_description: '',
        facultyCourse_duration: '',
        subtopic_name: '',
        subtopic_description: '',
        subtopic_video: '',
        subtopic_assignment: '',
        subtopic_assignment_totalMarks: '',
        file: [],
        subtopic_weightage: '',
        items: [],
        it: [],
        faculty_ta_require: [],
        headers: [
          {
            text: 'Name',
            align: 'left',
            sortable: false,
            value: 'subtopic_name'
          },
          {text: 'Description', value: 'subtopic_description'},
          {text: 'Video Link', value: 'subtopic_video'},
          {text: 'Assignment Link', value: 'subtopic_assignment'},
          {text: 'Weigtage ', value: 'subtopic_weightage'},
          {text: 'Total marks', value: 'subtopic_assignment_totalMarks: \'\',\n'}
        ],
        right: null
      }
    },

    computed: {},
    methods: {
      onFileSelected (event) {
        this.subtopic_assignment = event.target.files[0]
        console.log(this.subtopic_assignment)
      },
      onFileSelected2 (event) {
        this.facultyCourse_image = event.target.files[0]
        console.log(this.facultyCourse_image)
      },
      onFileSelected1 (event) {
        this.subtopic_video = event.target.files[0]
        console.log(this.subtopic_video)
      },
      detailcheck () {
        if (this.subtopic_video === '' ||
          this.facultyCourse_image === '' ||
          this.subtopic_assignment === '') {
          return false
        } else {
          return true
        }
      },
      save: function (subtopic_name, subtopic_description, subtopic_video, subtopic_assignment, subtopic_assignment_totalMarks, subtopic_weightage) {
        console.log(subtopic_video)
        if (this.detailcheck()) {
          this.facultyCourse_subtopics.push({
            subtopic_name: subtopic_name,
            subtopic_description: subtopic_description,
            subtopic_video: subtopic_video,
            subtopic_assignment: subtopic_assignment,
            subtopic_assignment_totalMarks: subtopic_assignment_totalMarks,
            subtopic_weightage: subtopic_weightage
          })
        } else {
          alert('Select Proper Data')
        }
        console.log(this.facultyCourse_subtopics)
      },
      del (subtopic_name, subtopic_description, subtopic_video, subtopic_assignment, subtopic_weightage, subtopic_assignment_totalMarks) {
        this.facultyCourse_subtopics.splice({
          subtopic_name: subtopic_name,
          subtopic_description: subtopic_description,
          subtopic_video: subtopic_video,
          subtopic_assignment: subtopic_assignment,
          subtopic_weightage: subtopic_weightage,
          subtopic_assignment_totalMarks: subtopic_assignment_totalMarks
        }, 1)
      },
      async getDetail () {
        console.log(Vue.localStorage.get('token'))
        var jwt = Vue.localStorage.get('token')
        console.log('view id called')
        if (jwt) {
          axios.get('http://192.168.137.1:3000/faculty/course/viewFacultyCourseInfo/' + this.$route.params.id,
            {
              headers: {
                'Authorization': 'bearer ' + Vue.localStorage.get('token')
              }
            })
            .then((response) => {
              console.log('hasgdfhj')
              console.log(response.data.subtopic)
              // this.items = response.data.subtopic
              this.it = response.data.course
              console.log(this.items)
              console.log(this.it)
            })
            .catch((error) => {
              console.log(error)
            })
        } else {
          this.$router.push('/faculty/login')
        }
      },
      updateCourse () {
        const fd = new FormData()
        console.log(Vue.localStorage.get('token'))
        var jwt = Vue.localStorage.get('token')
        console.log('view id called')
        if (this.detailcheck()) {
          if (jwt) {
            fd.append('facultyCourse_name', this.facultyCourse_name)
            fd.append('file[]', this.facultyCourse_image)
            fd.append('facultyCourse_prerequisites', this.facultyCourse_prerequisites)
            fd.append('faculty_ta_list', JSON.stringify(this.faculty_ta_list))
            fd.append('facultyCourse_description', this.facultyCourse_description)
            fd.append('facultyCourse_duration', this.facultyCourse_duration)
            fd.append('facultyCourse_subtopics', JSON.stringify(this.facultyCourse_subtopics))
            axios.patch('http://192.168.137.1:3000/faculty/course/update/' + this.$route.params.id, fd,
              {
                headers: {
                  'Content-type': 'multipart/form-data',
                  'Authorization': 'bearer ' + Vue.localStorage.get('token')
                }
              })
              .then(r => console.log('r: ', JSON.stringify(r, null, 2)))
              .catch(error => {
                console.log(error.response)
              })
            this.$router.push('/faculty/index')
          } else {
            this.$router.push('/faculty/login')
          }
        } else {
          alert('Enter Proper Data')
        }
      }
    },
    mounted () {
      this.getDetail()
    }
  }
</script>

