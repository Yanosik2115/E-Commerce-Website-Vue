<template>
	<div class="page-sign-up">
		<div class="columns">
			<div class="column is-4 is-offset-4">
				<h1 class="title">Sign up</h1>
				<form @submit.prevent="submitForm">
					<div class="field">
						<label class="label">Username</label>
						<div class="control">
							<input
								class="input"
								type="text"
								v-model="username"
								placeholder="Username"
							/>
						</div>
					</div>
					<div class="field">
						<label class="label">Password</label>
						<div class="control">
							<input
								class="input"
								type="password"
								v-model="password"
								placeholder="Password"
							/>
						</div>
					</div>
					<div class="field">
						<label class="label">Repeat password</label>
						<div class="control">
							<input
								class="input"
								type="password"
								v-model="password2"
								placeholder="Repeat password"
							/>
						</div>
					</div>
					<div class="notification is-danger" v-if="errors.length">
						<p v-for="error in errors" v-bind:key="error">
							{{ error }}
						</p>
					</div>
					<div class="field">
						<div class="control">
							<button class="button is-primary">Sign up</button>
						</div>
					</div>
				</form>
			</div>
		</div>
	</div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";

export default {
	name: "SignUp",
	data() {
		return {
			username: "",
			password: "",
			password2: "",
			errors: [],
		};
	},

	methods: {
		submitForm() {
			this.errors = [];

			if (this.username === "") {
				this.errors.push("Username is required");
			}
			if (this.password === "") {
				this.errors.push("Password is required");
			}
			if (this.password !== this.password2) {
				this.errors.push("The passwords doesn't match");
			}

			if (!this.errors.length) {
				const formData = {
					username: this.username,
					password: this.password,
				};

				axios
					.post("api/v1/users/", formData)
					.then((response) => {
						toast({
							message: "User created successfully",
							type: "is-success",
							dismissible: true,
							pauseOnHover: true,
							duration: 2000,
							position: "bottom-right",
						}),
							this.$router.push("/log-in");
					})
					.catch((error) => {
						if (error.response.data.username) {
							this.errors.push(error.response.data.username[0]);
						}
						if (error.response.data.password) {
							this.errors.push(error.response.data.password[0]);
						}
					});
			}
		},
	},
};
</script>
