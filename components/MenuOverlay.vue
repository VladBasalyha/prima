<template>
	<div id="MenuOverlay" class="fixed z-50 bottom-0 h-full w-full bg-gray-100 px-3">
		<div class="flex items-center justify-between py-5">
			<NuxtLink to="/" @click="userStore.isMenuOverlay = false">
				<img src="../public/icons/letter-p.png" width="100" alt="logo" />
			</NuxtLink>

			<button @click="userStore.isMenuOverlay = false" class="rounded-full p-2 hover:bg-gray-300">
				<Icon name="ic:sharp-cancel" size="40" />
			</button>
		</div>

		<div class="flex items-center justify-between pt-5">
			<ul class="w-full">
				<li @click="goTo('orders')" class="relative flex items-center justify-between py-2.5 border-b px-3 hover:bg-gray-300 cursor-pointer">
					<div class="flex items-center text-[20px] font-semibold">
						<Icon name="material-symbols:draft-orders-outline" size="33" />
						<span class="pl-4 font-extrabold">My orders</span>
					</div>
				</li>

				<li @click="goTo('shoppingCart')" class="relative flex items-center justify-between py-2.5 border-b px-3 hover:bg-gray-300 cursor-pointer">
					<div class="flex items-center text-[20px] font-semibold">
						<Icon name="clarity:shopping-bag-line" size="33" />
						<span class="pl-4 font-extrabold">Shopping cart</span>
					</div>

					<div class="flex items-center justify-center bg-gray-500 h-[35px] min-w-[35px] text-lg text-gray-100 rounded-full">
						{{ userStore.cart.length }}
					</div>
				</li>

				<li v-if="false" @click="signOut()" class="relative flex items-center justify-between py-2.5 border-b px-3 hover:bg-gray-300 cursor-pointer">
					<div class="flex items-center text-[20px] font-semibold">
						<Icon name="octicon:sign-out" size="33" />
						<span class="pl-4 font-extrabold">Sign out</span>
					</div>
				</li>

				<li v-else @click="signIn()" class="relative flex items-center justify-between py-2.5 border-b px-3 hover:bg-gray-300 cursor-pointer">
					<div class="flex items-center text-[20px] font-semibold">
						<Icon name="streamline:interface-share-user-human-person-share-signal-transmit-user" size="33" />
						<span class="pl-4 font-extrabold">Sign in / Register</span>
					</div>
				</li>
			</ul>
		</div>
	</div>
</template>

<script setup>
	import { useUserStore } from '~/stores/user';

	const userStore = useUserStore();

	// const client = useSupabaseClient();
	// const user = useSupabaseUser();

	const goTo = (url) => {
		userStore.isMenuOverlay = false;
		return navigateTo(`/${url}`);
	};

	const signOut = () => {
		client.auth.signOut();
		userStore.isMenuOverlay = false;
		return navigateTo('/');
	};

	const signIn = () => {
		userStore.isMenuOverlay = false;
		return navigateTo('/auth');
	};
</script>
