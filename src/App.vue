<template>
  <div id="app">
		<el-container>
			<el-main>
				<h1>Welcome to Repo Search</h1>
		<el-row>
			<div>
      	<el-button @click="popularJava">Click to see the most popular Java Repo</el-button>
    	</div>
		</el-row>
    <el-row>
			<el-col :span="6">
				<el-input v-model="con.lang"  placeholder="Enter a programming language"></el-input>
			</el-col>
  		<el-col :span="6">
				<el-select v-model="con.sortBy" placeholder="Select how you want to sort ">
					<el-option label="stars" value="stars"></el-option>
					<el-option label="forks" value="forks"></el-option>
					<el-option label="updated" value="updated"></el-option>
					<el-option label="help-wanted-issues" value="help-wanted-issues"></el-option>
				</el-select>
			</el-col>
			<el-col :span="6">
				<el-select v-model="con.sortOrder" placeholder="Select the sort order">
					<el-option label="ascending order" value="asc"></el-option>
					<el-option label="descending order" value="desc"></el-option>
				</el-select>
			</el-col>
  		<el-col :span="6">
				<el-button type="primary" @click="search(con)" icon="el-icon-search">Search</el-button>
			</el-col>
		</el-row>
		<el-row>
					<el-table
					v-if="showJava"
					:data="repoList"
					style="width: 100%"
					:cell-style="{'text-align': 'left'}"
					>
					<el-table-column
						prop="owner.login"
						label="Owner"
						width="180">
					</el-table-column>
					<el-table-column
						prop="name"
						label="Name"
						width="280">
					</el-table-column>
					<el-table-column
						prop="created_at"
						label="Created At"
						width="200">
					</el-table-column>
					</el-table>
					<el-table
					v-else
					:data="searchList"
					style="width: 100%"
					:cell-style="{'text-align': 'left'}"
					>
					<el-table-column
						prop="owner.login"
						label="Owner"
						width="180">
					</el-table-column>
					<el-table-column
						prop="name"
						label="Name"
						width="280">
					</el-table-column>
					<el-table-column
						prop="created_at"
						label="Created At"
						width="200">
					</el-table-column>
					<el-table-column
						prop="downloads_url"
						label="Download URL">
						<template slot-scope="scope">
						<a :href="'http://'+scope.row.downloads_url"
            target="_blank"
            class="buttonText">{{scope.row.downloads_url}}</a>
						 </template>
					</el-table-column>
					
					</el-table>
		</el-row>

			</el-main>
		</el-container>
  
	</div>
</template>

<script>
export default {
  data(){
    		return {
    			repoList: [],
					showJava: false,
					con:{
						lang:"",
						sortBy:"",
            sortOrder:""
					},
					searchList:[]
    		}
    	},
    	components: {
    		
    	},
    	mounted(){
    		this.initData();
    	},
  methods: {
		async initData(){
					console.log("start init ...");
					const response = await fetch("https://api.github.com/search/repositories?q=language:java&sort=stars&order=desc");
					const data = await response.json();
					data.items.forEach((v, i) => {
						this.repoList.push(v)
						console.log(v.owner.login + " / " +v.name + " - " + v.created_at);
					}
					);
    },
    popularJava () {
			this.showJava = !this.showJava;
    },
		async search(con) {
			
			const url = "https://api.github.com/search/repositories?q=language:" + con.lang + "&sort="+ con.sortBy + "&order=" + con.sortOrder;
			
			const response = await fetch(url);
			console.log(url);
			const data = await response.json();
			data.items.forEach((v, i) => {
				this.searchList.push(v)
				console.log(v);
			}
			);
			this.searchList.splice(1,0);
		}
  }
}
</script>

<style>
#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
}
.el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
}
.el-col {
  border-radius: 4px;
}
</style>
