<script >
    import { onMount } from 'svelte';
    /**
     * @type {any[]}
     */
    let users =[];
    let id = '';

// @ts-ignore
async function Getuserid(id) {
    const requestOptions = {
        method: "GET",
    };
    let url = `http://localhost:9999/api/user?id=${id}`;
    const response = await fetch(url, requestOptions);
    users = await response.json();
    console.log(users);
}


	
	</script>
<body  >


				<div class="search-box">
                <form class="inputsearch-box" action="/action_page.php">
                    
                    <input type="text" placeholder="SearchID.." class="inputsearch-field" name="search" bind:value={id}>
                    <div class="input-summit">
                        <button class="submit-btn" id="summit" type="button"><label for="summit" on:click={()=>Getuserid(id)}><span class="material-symbols-outlined">search</span></label></button>
                    </div>
                </form>
                   <div class="container">
                        <div class="teble-user">
                        <div class="teble-head">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th></th>
                                    <th>Name</th>
                                    <th>Username</th>
                                    <th>Email</th>
                                    <th>Phone Number</th>
                                    <th>Id Card</th>
                                    <th></th>
                                    <th></th>
                                </tr>
                            </thead>
                           <tbody>
                                {#each users as user}
                                    <tr>
                                        <td>{user.id}</td>
                                        <td>{user.name} </td>
                                        <td>{user.username}</td>
                                        <td>{user.email}</td>
                                        <td>{user.phone_number}</td>
                                        <td>{user.id_card}</td>
                                    </tr>
                                {/each}
                            </tbody>
                        </table>
                        </div>
                        </div>
                    </div>


				</div>

        </body>
	<style>
		*{
			margin: 0;
			padding: 0;
			box-sizing: border-box;
			font-family: 'Poppins', sans-serif;
		}
		body{
			display: flex;
			justify-content: center;
			align-items: center;
			min-height: 100vh;
			background-image: url("/src/routes/img/BG1.jpg");
			background-size: cover;
			background-position: center;
		}

        .container{
            background: #cba3e6;
        }

		.search-box{
			font-size: 20px;
			text-align: center;
			color: #061750;
            width: 1500px;
			background: transparent;
			border: 1px solid #727272;
			backdrop-filter: blur(20px);
			box-shadow: 0 0 10px #1e1f1e;
			color: #fff;
			border-radius: 20px;
			padding: 90px 90px;
		}
		.inputsearch-box .inputsearch-field{
			width: 100%;
			height: 65px;
			font-size: 18px;
			padding: 0 25px;
			margin-bottom: 20px;
			border-radius: 30px;
			border:none;
			box-shadow: 0px 5px 50px 1px #b1b3bb;
			outline: none;
			transition: 3s;
		}
        .submit-btn{
			width: 150px;
			height: 55px;
			background: #e2dada;
			border: none;
			outline: none;
            font-size: 30px;
			border-radius: 30px;
			box-shadow: 0 0 20px rgb(121, 120, 120);
		}
	
		::placeholder{
			font-weight: 100%;
			color:#333534;
		}
		.inputsearch-field:focus{
			width: 105%;
		}
        .teble-user{
            top: 20px;
            background: #94bfe2;
        }
        .teble-head{
            padding: 0;
            background: transparent;
            border: 1px solid #727272;
            backdrop-filter: blur(20px);
            box-shadow: 0 0 10px #3a3d3a;
            color: #fff;
            border-radius: 10px;
            padding: 30px 40px;
        }
        main {
            max-width: 600px;
            margin: 0 auto;
            padding: 1em;
        }
        div {
            margin-bottom: 1em;
        }
        label {
            display: block;
            margin-bottom: 0.5em;
        }
        input {
            width: 100%;
            padding: 0.5em;
            box-sizing: border-box;
        }

	</style>