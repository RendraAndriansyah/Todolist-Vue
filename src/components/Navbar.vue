<template lang="">
	<div>
		<nav class="navbar bg-body-tertiary bg-secondary">
			<div class="container">
				<a class="navbar-brand text-white" href="#">{{ appName }}</a>
			</div>
		</nav>
	</div>
</template>
<script>
export default {
	props: {
		appName: String,
	},
};
</script>
