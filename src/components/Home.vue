<template>
  <div id="" class="container">
    <!-- set progress bar -->
    <vue-progress-bar></vue-progress-bar>
    <div class="row">
      <div class="col-md-6 mx-auto">
        <h1 class="text-center">Myanmar Sentiment Classification</h1>
        <form v-on:submit.prevent="postText">
          <label for="textInput">Sentiment Classification</label>
          <input id="textInput" v-model="inputText" type="text" class="form-control" placeholder="Enter Text">
          <button type="submit" class="btn btn-success btn-block mt-3">Submit</button>
        </form>
        <table class="table">
          <tr>
            <td class="font-weight-bold text-center">Text</td>
            <td class="font-weight-bold text-center">Prediction</td>
          </tr>
          <tr v-for="(txt) in predictionList" v-bind:key="txt.id" v-bind:text="txt.text" v-bind:label="txt.label">
            <td class="text-center">{{txt.text}}</td>
            <td class="text-center">{{txt.label}}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script type="text/javascript">
import axios from 'axios'

export default {
  name: 'Home',
  data () {
    return {
      predictionList: [],
      id: '',
      inputText: '',
      isEdit: false
    }
  },
  mounted () {
    this.getList()
  },
  methods: {
    start () {
      this.$Progress.start()
    },
    set (num) {
      this.$Progress.set(num)
    },
    increase (num) {
      this.$Progress.increase(num)
    },
    decrease (num) {
      this.$Progress.decrease(num)
    },
    finish () {
      this.$Progress.finish()
    },
    fail () {
      this.$Progress.fail()
    },
    getList () {
      axios({method: 'GET', url: 'https://mmsentiment-api.herokuapp.com/api/prediction'}).then(
        result => {
          console.log(result.data)
          this.predictionList = result.data
        },
        error => {
          console.error(error)
        }
      )
    },
    postText () {
      this.$Progress.start()
      axios.post('https://mmsentiment-api.herokuapp.com/api/prediction', {text: this.inputText}).then(
        res => {
          this.inputText = ''
          this.getList()
          console.log(res)
          this.$Progress.finish()
        },
        res => {
          this.$Progress.fail()
        }
      ).catch(
        err => {
          console.log(err)
        }
      )
    }
  }
}

</script>
