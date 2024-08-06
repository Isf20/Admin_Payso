<script lang="ts">
	
	// @ts-nocheck

		let username = "";
		let password = "";
	const Sigindata = async () => {
			console.log("login");
			console.log("username:", username);
			
			let config = {
				method: 'Post',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					username: username,
					password: password
				})
			};
	
			var result = await fetch(`http://localhost:9999/api/login`, config);
			var data = await result.json();
			console.log(data);
			
			if (data.length == 0 ){
				console.log('User not found')
	
			}else{
				console.log('')
				window.location.href = '/userall'
			}		
		};
	
	</script>
