<template>
	<div class="container">
		<div class="row">
			<div class="col-sm-6 offset-3">

				<div v-if="sessionStarted" id="chat-container" class="card">
					<div class="card-header text-white text-center font-weight-bold subtle-blue-gradient">
						Share the page URL to invite new friends
					</div>

					<div class="card-body">
						<div class="container chat-body">
							<div v-for="message in messages" :key="message.id" class="row chat-section">
								<template v-if="username === message.user.username">
									<div class="col-sm-7 offset-3">
										<span class="card-text speech-bubble speech-bubble-user float-right text-white subtle-blue-gradient">
											{{ message.message }}
										</span>
									</div>
									<div class="col-sm-2">
										<img class="rounded-circle" :src="`http://placehold.it/40/007bff/fff&text=${message.user.username[0].toUpperCase()}`" />
									</div>
								</template>
								<template v-else>
									<div class="col-sm-2">
										<img class="rounded-circle" :src="`http://placehold.it/40/333333/fff&text=${message.user.username[0].toUpperCase()}`" />
									</div>
									<div class="col-sm-7">
										<span class="card-text speech-bubble speech-bubble-peer">
											{{ message.message }}
										</span>
									</div>
								</template>
							</div>
						</div>
					</div>

					<div class="card-footer text-muted">
						<form>
							<div class="row">
								<div class="col-sm-10">
									<input type="text" placeholder="Type a message" />
								</div>
								<div class="col-sm-2">
									<button class="btn btn-primary">Send</button>
								</div>
							</div>
						</form>
					</div>
				</div>

				<div v-else>
					<h3 class="text-center">Welcome !</h3>
					<br />
					<p class="text-center">
						To start chatting with friends click on the button below, it'll start a new chat session
						and then you can invite your friends over to chat!
					</p>
					<br />
					<button @click="startChatSession" class="btn btn-primary btn-lg btn-block">Start Chatting</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
var axios = require('axios');

export default {
  data () {
    return {
      sessionStarted: false,
      messages: [
        {"status":"SUCCESS","uri":"040213b14a02451","message":"Hello!","user":{"id":1,"username":"danidee","email":"osaetindaniel@gmail.com","first_name":"","last_name":""}},
        {"status":"SUCCESS","uri":"040213b14a02451","message":"Hey whatsup! i dey","user":{"id":2,"username":"daniel","email":"","first_name":"","last_name":""}}
      ]
    }
  },

	created() {
		this.username = sessionStorage.getItem('username');
	},

	methods: {
		startChatSession() {
			let router = this.$router;
			let sessionStarted = this.sessionStarted;

			axios.create({
							headers: {
								'Authorization': `Token ${sessionStorage.getItem('authToken')}`
							}
					 })
					 .post('http://localhost:8000/api/chats/')
					 .then(function(response) {
					 		console.log(response);
							sessionStarted = true;
							router.push(`/chats/${response.data.uri}/`);
					 })
					 .catch(function(error) {
							console.log(error);
					 })
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
	font-weight: normal;
}
ul {
	list-style-type: none;
	padding: 0;
}
li {
	display: inline-block;
	margin: 0 10px;
}

.btn {
	border-radius: 0 !important;
}

.card-footer input[type="text"] {
	background-color: #ffffff;
	color: #444444;
	padding: 7px;
	font-size: 13px;
	border: 2px solid #cccccc;
	width: 100%;
	height: 38px;
}

.card-header a {
	text-decoration: underline;
}

.card-body {
	background-color: #ddd;
}

.chat-body {
	margin-top: -15px;
	margin-bottom: -5px;
	height: 380px;
	overflow-y: auto;
}

.speech-bubble {
	display: inline-block;
	position: relative;
	border-radius: 0.4em;
	padding: 10px;
	background-color: #fff;
	font-size: 14px;
}

.subtle-blue-gradient {
	background: linear-gradient(45deg,#004bff, #007bff);
}

.speech-bubble-user:after {
	content: "";
	position: absolute;
	right: 4px;
	top: 10px;
	width: 0;
	height: 0;
	border: 20px solid transparent;
	border-left-color: #007bff;
	border-right: 0;
	border-top: 0;
	margin-top: -10px;
	margin-right: -20px;
}

.speech-bubble-peer:after {
	content: "";
	position: absolute;
	left: 3px;
	top: 10px;
	width: 0;
	height: 0;
	border: 20px solid transparent;
	border-right-color: #ffffff;
	border-top: 0;
	border-left: 0;
	margin-top: -10px;
	margin-left: -20px;
}

.chat-section:first-child {
	margin-top: 10px;
}

.chat-section {
	margin-top: 15px;
}

.send-section {
	margin-bottom: -20px;
	padding-bottom: 10px;
}
</style>