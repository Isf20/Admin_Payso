<script>
	import Sidebar from "../../lib/Component/Sidebar/+page.svelte";
	import Navbar from "../../lib/Component/Navbar/+page.svelte";
	let username = "";
	let password = "";

	const FetchData = async () => {
		let token = sessionStorage.getItem('authToken');
		console.log(token)
	};
</script>



<svelte:head>
	<title>About</title>
	<meta name="description" content="About this app" />
</svelte:head>
<div class="w-100 min-vh-100 bg-secondary-subtle">
	<Navbar/>
	<div class="d-flex">
	<Sidebar/>
	<div class="w-100 overflow-scroll p-3">
		<span>aaa</span>
		<button class="btn btn-primary" on:click={()=>FetchData()}> getauth</button>
	</div>
</div>
</div>

<style>

</style>