<template>
  <div>
    <div class="section">
    <div class=name> {{name}}</div>
    <div class=type-container> 
      <input type=radio :checked="value.type=='rows'" @click="set_type('rows')"> Rows
      <input type=radio :checked="value.type=='raw'" @click="set_type('raw')"> Raw
      <input type=radio :checked="value.type=='object'" @click="set_type('object')"> Object
    </div>
    <textarea v-if="type=='rows'" v-model="rows"/>
    <span v-if="type=='rows'"> Length: {{count('rows')}} </span>
    <div v-if="type=='object'" class="object-container">
      <div>
        <textarea v-model="keys"/>
        Length: {{count('keys')}}
      </div>
      <div>
        <textarea v-model="values"/>
        Length: {{count('values')}}
      </div>
    </div>
    <textarea v-bind:value="json" @input="set_json(JSON.parse($event.target.value))" />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'Section',
  props: {
    value: Object,
    name: String,
  },
  methods: {
    set_json(json) {
        this.$emit('input', {type: this.value.type, json})
    },
    set_type(value) {
        if(this.value.type === 'rows' && value === 'object') {
          this.value.json = _.zipObject(_.range(this.value.json.length), this.value.json)
        }
        if(this.value.type === 'object' && value === 'rows') {
          this.value.json = _.values(this.value.json)
        }
        this.value.type = value;
        console.log(this.value)
        this.$emit('input', this.value)      
    },
    count(item){
      return _.get(this, item, '').split('\n').length;
    },
  },
  computed: {
    rowsLength() {return _.get(this, 'rows', '').split('\n').length},    
    rowsLength() {return _.get(this, 'rows', '').split('\n').length},    
    rowsLength() {return _.get(this, 'rows', '').split('\n').length},    
    type: {
      get() {
        return this.value.type;
      },
      set(value) {
        this.set_type(value);
      },
    },
    json: {
      get() {
        return JSON.stringify(this.value.json, null, 2);
      },
      set(value) {
//        console.log(value)
//        this.set_json(value);
      },
    },
    rows: {
      get() {
        try {
          return this.value.json.join('\n');
        } catch(e) {
          //return 'Error ' + JSON.stringify(e)
        }
      },
      set(value) {
        this.set_json(value.split('\n'))
      },
    },
    keys: {
      get() {
        return Object.keys(this.value.json).join('\n');
      },
      set(value) {
        this.set_json(_.zipObject(value.split('\n'), _.values(this.value.json)))
      },
    },
    values: {
      get() {
        return Object.values(this.value.json).join('\n');
      },
      set(value) {
        this.set_json(_.zipObject(_.keys(this.value.json), value.split('\n')))
      },
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.section {
  display: flex;
  flex-direction: column;
}
.name {
  font-size: 18px;
  font-weight: bold;
}
textarea {
  width: 350px;
  height: 180px;
  padding: 10px;
  margin: 10px;
}
.object-container{
  display: flex;
  flex-direction: row;
}
div.object-container textarea {
  width: 150px;
}
</style>
