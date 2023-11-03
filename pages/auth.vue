<template>
	<div id="AuthPage" class="w-full h-[100vh] bg-white">
		<div class="w-full flex items-start justify-center p-4 border-b border-b-gray-400">
			<NuxtLink to="/">
				<img width="70" height="70" class="block mr-auto" src="../public/icons/letter-p.png" alt="logo" />
			</NuxtLink>
		</div>
		{{ user }}
		<div class="max-w-[400px] mx-auto px-2">
			<div class="text-center my-4 font-bold uppercase">Login / Register</div>

			<button
				@click="login('google')"
				class="block mx-auto bg-gray-100 justify-center gap-3 p-1.5 w-[60%] border hover:bg-gray-200 rounded-full text-lg font-semibold">
				<div class="flex items-center justify-center gap-2">
					<Icon name="devicon:google" />
					<div class="text-gray-800">Google</div>
				</div>
			</button>

			<button
				@click="login('github')"
				class="block mt-4 mx-auto bg-gray-100 justify-center gap-3 p-1.5 w-[60%] border hover:bg-gray-200 rounded-full text-lg font-semibold">
				<div class="flex items-center justify-center gap-2">
					<Icon name="mdi:github" />
					<div class="text-gray-800">Github</div>
				</div>
			</button>
		</div>
	</div>
</template>

<script setup>
	const client = useSupabaseClient();
	const user = useSupabaseUser();

	watchEffect(() => {
		if (user.value) {
			navigateTo('/');
		}
	});

	const login = async (prov) => {
		const { data, error } = await client.auth.signInWithOAuth({
			provider: prov,
		});
	};
</script>
