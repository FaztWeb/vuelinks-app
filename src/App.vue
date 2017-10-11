<template>
	<div id="app" class="container">
		<h1>Vue and Firebase</h1>
		<hr>
		
		
		<div class="card">
			<div class="card-header">
				<h3>Add a Link</h3>
			</div>
			<div class="card-body">
				<form v-on:submit.prevent="addLink"  class="form-inline">

					<div class="form-group">
						<label for="">Title</label>
						<input
							placeholder="Title"
							v-model="newLink.title"
							type="text" class="form-control mx-sm-4"/>
					</div>
					
					<div class="form-group">
						<label for="">Author</label>
						<input
							placeholder="Author"
							v-model="newLink.author"
							type="text" class="form-control mx-sm-4"/>
					</div>
					
					<div class="form-group">
						<label for="">Url</label>
						<input
							placeholder="Title"
							v-model="newLink.url"
							type="text" class="form-control mx-sm-4"/>
					</div>

					<input type="submit" class="btn btn-success" value="Add Link">
				</form>
			</div>
		</div>

		<hr>

		<div class="card">
			<div class="card-header">
				<h3 class="card-title">Links List</h3>
			</div>
			<div class="card-block">
				<table class="table table-striped">
					<thead>
						<tr>
							<th>Title</th>
							<th>User</th>
							<th>Delete</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="link in links">
							<td>
								<a v-bind:href="link.url">
									{{ link.title }}
								</a>
							</td>
							<td>{{ link.author }}</td>
							<td>
								<button 
									v-on:click="deleteLink(link)"
									class="btn btn-danger">
									<i class="fa fa-trash-o" aria-hidden="true"></i>
								</button>									
							</td>
						</tr>
					</tbody>
				</table>
			</div>
		</div>
	</div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase';
import toastr from 'toastr';

let config = {
	Key: "AIzaSyAmGkyujxtcaJQRUN3mCd_eZwBbOumBZds",
	    authDomain: "firevue-dfff5.firebaseapp.com",
	    databaseURL: "https://firevue-dfff5.firebaseio.com",
	    projectId: "firevue-dfff5",
	    storageBucket: "firevue-dfff5.appspot.com",
	    messagingSenderId: "306892195263"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let linksRef = db.ref('links');

export default {
	name: 'app',
	firebase: {
		links: linksRef
	},
	data() {
		return {
			newLink: {
				title: '',
				author: '',
				url: ''
			}
		}
	},
	methods: {
		addLink: function() {
			linksRef.push(this.newLink);
			this.newLink.title = '';
			this.newLink.author = '';
			this.newLink.url = '';
		},
		deleteLink: function(link) {
			linksRef.child(link['.key']).remove();
			toastr.success('Link removed');
		}
	}
}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
