<template>
  <div>
    <div class=state> Save
      <input type=button value=A @click="save_state">
      <input type=button value=B @click="save_state">
      <input type=button value=C @click="save_state">
    Load
      <input type=button value=A @click="load_state">
      <input type=button value=B @click="load_state">
      <input type=button value=C @click="load_state">
    </div>
  	<div class=migration> 
  		Formula: <input type=text class=formula v-model='formula' @input="save('formula', $event.target.value)">
  		<select class=formula v-model=formula @change="save('formula', $event.target.value)">
  			<option v-for="formula, name in FORMULAS" :value="formula"> {{name}} </option>
  		</select>
  	</div>
	<div class="sections">
	  <Section name="Input A" v-model="A" @input="save('A', $event)"/>
	  <Section name="Input B" v-model="B" @input="save('B', $event)"/>
	  <Section name="Result" v-model="result"/>
	</div>
  </div>
</template>

<script>
// @ is an alias to /src
import Section from '@/components/Section.vue'
import _ from 'lodash'

export default {
  name: 'Tools',
  created() {
   var basics = ['intersection', 'difference','merge','union','zipObject'];
	 this.FORMULAS = _.assign({
      "find_duplicates" : "_(A).groupBy().pickBy(x => x.length > 1).keys().value()",
   }, _.zipObject(basics, basics.map(item => `_.${item}(A,B)`)));
   
  },
  data() {
  	return _.mapValues({
  			formula: '_.difference(A,B)',
  			A: { json: [], type:'rows' },
  			B: { json: [], type:'rows' },
  			result: { json: [], type:'rows' },
  		}, (def, variable) => this.load(variable, def)
  	);
  },
  components: {
    Section
  },
  methods: {
  	save(id, value){
  		console.log(id, value)
      window[id] = value.json
  		localStorage[id] = JSON.stringify(value);
  		this.result = this.calc_result(this.A.json, this.B.json, this.formula);
  	},
    save_state(e){
      if(e.target.value) {
        localStorage['slot-'+e.target.value] = JSON.stringify(_.pick(this, ["formula", "A", "B"]));
      }
    },
    load_state(e){
      var data = localStorage['slot-'+e.target.value];
      data && _.assign(this, JSON.parse(data));

    },
  	load(id, def) {
  		try { return JSON.parse(localStorage[id]) }
  		catch(e) {return _.cloneDeep(def)};
  	},
  	calc_result(A, B, formula){
  		var value = eval(formula);
  		return {
  			json: eval(formula),
  			type: value instanceof Array ? 'rows' : 'object'
  		}
  	},
  },
}
</script>
<style scoped> 
.sections {
	display: flex;
	flex-direction: row;
	justify-content: space-between;
}
select {
	margin: 5px;
}
input.formula {
  width: 600px;
}
.formula,{
  font-size: 18px;
}
.migration {
  padding-bottom: 15px;
  padding-top: 15px;
  margin-left: 120px;
  margin-right: 120px;
  margin-bottom: 15px;
  font-size: 20px;
  font-weight: bold;
  border: 2px dashed black;
}
input[type=button] {
  font-size: 24px;
  margin: 10px;
}
.state  {
   font-weight: bold;
   font-size: 24px;
}
</style>