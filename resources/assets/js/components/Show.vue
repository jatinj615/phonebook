<template>
	<div class="modal" :class='openmodal'>
	  <div class="modal-background"></div>
	  <div class="modal-card">
	    <header class="modal-card-head">
	      <p class="modal-card-title">{{ list.name }}'s Details</p>
	      <button class="delete" aria-label="close" @click="close"></button>
	    </header>
	    <section class="modal-card-body">

	     	<li class="panel-block">
	     		<label class="column is-2"><b>Name</b></label> {{ list.name }}
	     	</li>
			<li class="panel-block">
				<label class="column is-2"><b>Phone</b></label> {{ list.phone }}
			</li>		
			<li class="panel-block">
				<label class="column is-2"><b>Email</b></label> {{ list.email }}
			</li>
	

	     	<!-- <div class="field">
	     	  <label class="label">Name</label>
	     	  <div class="control">
	     	    <p>{{ list.name }}</p>
	     	  </div>
	     	</div>
	     	<div class="field">
	     	  <label class="label">Phone</label>
	     	  <div class="control">
	     	    <p>{{ list.phone }}</p>
	     	  </div>
	     	  
	     	</div>
	     	<div class="field">
	     	  <label class="label">Email</label>
	     	  <div class="control">
	     	    <p>{{ list.email }}</p>
	     	  </div>
	     	  
	     	</div> -->
	    </section>
	    <footer class="modal-card-foot">
	      
	      <button class="button" @click="close">Cancel</button>
	    </footer>
	  </div>
	</div>
</template>

<script>
	export default{
		props:['openmodal'],
		data(){
			return{
				list:''
			}
		},
		methods:{
			close(){
				this.$emit('closeRequest')
			},
		}
	}
</script>