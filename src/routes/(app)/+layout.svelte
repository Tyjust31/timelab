<script lang="ts">
	//import '../app.css';
	
	let { children } = $props();
	import Footer from "./footer.svelte";
	import Header from "./header.svelte";
</script>

<Header></Header>

{@render children()}

<Footer></Footer>
