<template>
	<div>
		<nav class="panel column is-offset-2 is-8">
		  <p class="panel-heading">
		    Phone Book
		    <button class="button is-link is-outlined" @click="openAdd">
		      Add new
		    </button>
		    <span class="is-pulled-right" v-if="loading">
		    	<i class="fa fa-circle-o-notch fa-spin fa-1x fa-fw"></i>
		    </span>
		  </p>
		  <div class="panel-block">
		    <p class="control has-icons-left">
		      <input class="input is-small" type="text" placeholder="search" v-model="searchQuery">
		      <span class="icon is-small is-left">
		        <i class="fa fa-search"></i>
		      </span>
		    </p>
		  </div>
		  <a class="panel-block is-active" v-for="item,key in temp">
		    <span class="column is-9">
		    	{{ item.name }}
		    </span>
		    <span class="panel-icon column is-1">
		      <i class="has-text-danger fa fa-trash" aria-hidden="true" @click="del(key,item.id)"></i>
		    </span>
		    <span class="panel-icon column is-1">
		      <i class="has-text-info fa fa-edit" aria-hidden="true" @click="openUpdate(key)"></i>
		    </span>
		    <span class="panel-icon column is-1">
		      <i class="has-text-primary fa fa-eye" aria-hidden="true" @click="openShow(key)"></i>
		    </span>
		  </a>
		</nav>

		<Add :openmodal='addActive' @closeRequest="close"></Add>
		<Show :openmodal='showActive' @closeRequest="close"></Show>
		<Update :openmodal='updateActive' @closeRequest="close"></Update>
	</div>
</template>
	<script>
		let Add = require('./Add.vue');
		let Show = require('./Show.vue');
		let Update = require('./Update.vue');
		export default{
			components:{Add, Show, Update},
			data(){
				return{
					addActive:'',
					showActive:'',
					updateActive:'',
					list:{},
					errors:{},
					loading:false,
					searchQuery:'',
					temp:''
				}
			},
			mounted(){
				axios.post('/getData').then((response)=>this.list = this.temp = response.data)
				  .catch((error)=>this.errors = error.response.data.errors);
			},
			watch:{
				searchQuery(){
					if(this.searchQuery.length > 0) {
						this.temp = this.list.filter((item) => {
							return Object.keys(item).some((key)=>{
								let string = String(item[key])
								// console.log(string)
								return string.toLowerCase().indexOf(this.searchQuery.toLowerCase())>-1
							})
						});
					} else{
						this.temp = this.list
					}
				}
			},
			methods:{
				openAdd(){
					this.addActive = 'is-active'
				},
				openShow(key){
					this.$children[1].list = this.temp[key]
					this.showActive = 'is-active'
				},
				openUpdate(key){
					this.$children[2].list = this.temp[key]
					this.updateActive = 'is-active'
				},
				close(){
					this.addActive = '',
					this.showActive = '',
					this.updateActive = ''
				},
				del(key, id){
					if(confirm("Are You Sure")){
						this.loading = !this.loading
						axios.delete(`/phonebook/${id}`).then((response)=>{this.list.splice(key,1);this.loading = !this.loading})
						  .catch((error)=>this.errors = error.response.data.errors);	
					}
				}
			}
			
		}
	</script>