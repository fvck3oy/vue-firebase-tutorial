
<template>

  


  <div class="hello">
    

    
    
  <h2>{{ msg }}   {{currentUser}}</h2>

     <div id="new-employee">

    <div class="row">

      <div>
   <ul class="ull">
      <h1> <a href="/#/order" target=""><i class="fas fa-plus-circle"></i>สั่งอาหาร </a></h1>
      <li class=""><h2>List Order</h2></li>
<li v-for="customer in customer" v-bind:key="customer.id" >

       <div class="c0">

              ผู้สั่ง : {{customer.customer}}   <br/>
              อาหาร : {{customer.name}} <br/>
                {{customer.ea}} จำนวน
<hr class="hrr" />
       </div>
      </li><br />
    </ul>
      </div>


  </div>
  </div>


    
 
    <h2> <a href="/#/profile" target=""><i class="fas fa-users"></i><br />ไปหน้าผู้จัดทำ</a></h2>

    <button v-on:click="logout" class="btnLogout">Logout</button>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'
export default {
	name: 'new-customer',

	// name: 'hello',
	data() {
		return {
			msg: 'ยินดีต้อนรับคุณ',
			customer: [],
			loading: true
		}
	},
	created() {
		db
			.collection('order')
			// .orderBy('')
			.get()
			.then(querySnapshot => {
				this.loading = false
				querySnapshot.forEach(doc => {
					const data = {
						id: doc.id,
						customer: doc.data().customer,
						name: doc.data().name,
						ea: doc.data().ea
					}
					this.customer.push(data)
					console.log('data ', this.customer)
				})
			})

		if (firebase.auth().currentUser) {
			this.isLoggedIn = true
			this.currentUser = firebase.auth().currentUser.email
		}
	},
	watch: {
		$route: 'fetchData'
	},
	methods: {
		// saveEmployee() {
		// 	db
		// 		.collection('order')
		// 		.add({
		// 			// employee_id: this.employee_id,
		// 			order: this.name,
		//       ea: this.ea,
		//       customer: this.currentUser
		// 			// dept: this.dept,
		// 			// position: this.position
		// 		})
		// 		.then(docRef => {
		// 			console.log('Client added: ', docRef.id)
		//       this.$router.push('/profile')

		// 		})
		// 		.catch(error => {
		// 			console.error('Error adding employee: ', error)
		// 		})
		// },
		logout: function() {
			firebase
				.auth()
				.signOut()
				.then(() => {
					this.$router.replace('login')
				})
		}
	}
}
</script>


<style scoped>
h1,
h2 {
	font-weight: normal;
}
.ull {
	display: grid;
}
ul {
	list-style-type: none;
	padding: 0;
}

li {
	display: inline-block;
	margin: 0 10px;
}

a {
	color: #42b983;
}
ul {
	/* list-style-type: none; */
	margin: 0;
	padding: 0;
	overflow: hidden;
	/* background-color: #333; */
}

li {
	float: left;
}

li a {
	display: block;
	color: white;
	text-align: center;
	padding: 14px 16px;
	text-decoration: none;
}

li a:hover:not(.active) {
	background-color: #111;
}

.active {
	background-color: #4caf50;
}

.hello {
	text-align: center;
}

.hrr{
  width: 500px;
}
.c0 {
	width: auto;
  margin-top: 10px;
  margin-bottom: 10px;
	/* border: 1px solid #000; */
}

.btnLogout{
color: rgb(55, 123, 52);
    font-size: 18px;
    width: 250px !important;
    height: 40px;
    border-radius: 20px !important;
    border: 0.5px solid rgb(82, 212, 74);
    padding: 4px 0px 4px 0px;
    box-shadow: rgb(75, 197, 96) 3px 3px 10px -3px;
    outline-width: 0px;
    cursor: pointer;
    margin-bottom: 20px;
}

.btnLogout:hover{
color: #fff;
background-color: #4caf50;
}
</style>
