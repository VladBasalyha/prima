<script setup>
	import Loading from '~/components/Loading.vue';
	import { useUserStore } from '../stores/user';

	const client = useSupabaseClient();
	const user = useSupabaseUser();

	let isAccountMenu = ref(false);
	let searchItem = ref('');
	let items = ref(null);
	let isSearching = ref(false);
	let isCartHover = ref(false);

	const userStore = useUserStore();

	const searchByName = useDebounce(async () => {
		isSearching.value = true;
		items.value = await useFetch(`/api/prisma/search-by-name/${searchItem.value}`);
		isSearching.value = false;
	}, 100);

	watch(
		() => searchItem.value,
		async () => {
			if (!searchItem.value) {
				setTimeout(() => {
					items.value = '';
					isSearching.value = false;
					return;
				}, 500);
			}
			searchByName();
		},
	);
</script>

<template>
	<div id="MainLayout" class="w-full fixed z-50">
		<div id="TopMenu" class="w-full bg-gray-500 border-b md:block hidden">
			<ul class="flex items-center justify-end text-xs text-white font-medium px-6 h-10 bg-gray-500 max-w-[1200px]">
				<li class="border-r-gray-200 cursor-pointer px-3 hover:text-[#71c7ec]">Prima Market</li>
				<li class="border-r-gray-200 cursor-pointer px-3 hover:text-[#71c7ec]">Cookies</li>
				<li class="border-r-gray-200 cursor-pointer px-3 hover:text-[#71c7ec]">Support</li>
				<li class="border-r-gray-200 cursor-pointer px-3 hover:text-[#71c7ec]">Buyer protection</li>
				<li class="border-r-gray-200 align-middle cursor-pointer px-3 hover:text-[#71c7ec]">
					<Icon name="material-symbols:smartphone-outline-sharp" size="17" />
					App
				</li>
				<li
					@mouseenter="isAccountMenu = true"
					@mouseleave="isAccountMenu = false"
					class="relative ml-2 flex items-center h-[90%] cursor-pointer hover:text-[#71c7ec]"
					:class="isAccountMenu ? '' : ''">
					<Icon name="material-symbols:account-circle-full" size="17" />
					<Icon name="material-symbols:arrow-drop-down" size="15" class="ml-1" />
					<div id="AccountMenu" v-if="isAccountMenu" class="absolute bg-gray-500 w-[220px] text-gray-300 z-40 top-[35px] right-[-9px] border-x border-b">
						<div v-if="!user">
							<div class="text-center text-semibold text-[15px] my-4 px-3">Prima likes to see you</div>
							<div class="flex items-center gap-1 px-3 mb-3">
								<NuxtLink to="/auth" class="bg-gray-300 text-center w-full text-[16px] rounded-sm text-gray-800 font-semibold p-2"
									>Login/Register</NuxtLink
								>
							</div>
						</div>
						<div class="border-b">
							<ul class="bg-gray-300">
								<li class="text-center text-gray-700 text-[16px] py-2 px-4 w-full hover:bg-gray-200" @click="navigateTo('/orders')">
									Orders
								</li>

								<li
									v-if="user"
									@click="client.auth.signOut()"
									class="text-center text-gray-400 text-[16px] py-2 px-4 w-full hover:bg-gray-200">
									Sign out
								</li>
							</ul>
						</div>
					</div>
				</li>
			</ul>
		</div>

		<div id="MainHeader" class="flex justify-center items-center w-full bg-gray-200">
			<div class="flex items-center justify-between gap-10 max-w-[1150px] w-full px-6 lg:justify-start">
				<NuxtLink to="/" class="min-w-[170px]">
					<img width="100" src="../public/icons/letter-p.png" alt="" />
				</NuxtLink>

				<div class="w-full md:block hidden max-w-[700px]">
					<div class="relative bg-gray-400">
						<div class="flex items-center border-2 border-gray-400 rounded-md w-full">
							<input
								class="w-full bg-gray-400 placeholder-gray-200 text-sm pl-3 focus:outline-none"
								placeholder="search"
								v-model="searchItem"
								type="text" />

							<Icon v-if="isSearching" class="mr-2" name="eos-icons:loading" size="25" />
							<button class="flex bg-gray-500 items-center h-[100%] p-1.5 px-2">
								<Icon name="mdi:archive-search-outline" size="20" color="#ffffff" />
							</button>
						</div>

						<div class="absolute bg-gray-300 h-auto w-full max-w-[700px]">
							<div v-if="items && items.data" v-for="item in items.data" class="p-1">
								<NuxtLink class="flex items-center justify-between w-full cursor-pointer hover:bg-gray-500" :to="`/item/${item.id}`">
									<div class="flex items-center">
										<img class="rounded-md" width="40" :src="item.url" alt="product-image" />
										<div class="truncate ml-2">{{ item.title }}</div>
									</div>
									<div class="truncate text-md font-semibold">{{ item.price / 100 }}</div>
								</NuxtLink>
							</div>
						</div>
					</div>
				</div>
				<NuxtLink to="/shoppingcart" class="flex items-center">
					<button class="relative md:block hidden" @mouseenter="isCartHover = true" @mouseleave="isCartHover = false">
						<span
							class="absolute flex items-center justify-center -right-[3px] top-0 bg-gray-500 h-[15px] min-w-[17px] text-xs text-gray-200 py-2 px-1 rounded-full">
							{{ userStore.cart.length }}
						</span>
						<Icon name="emojione-monotone:shopping-cart" size="25" :color="isCartHover ? '#71c7ec' : ''" />
					</button>
				</NuxtLink>

				<button @click="userStore.isMenuOverlay = true" class="md:hidden block rounded-full p-1.5 -mt-[4px] hover:bg-gray-400">
					<Icon name="ic:outline-menu-open" size="33" />
				</button>
			</div>
		</div>
	</div>
	<Loading v-if="userStore.isLoading" />
	<div class="lg:pt-[150px] md:pt-[130px] pt-[80px]">
		<slot />
		<Footer v-if="!userStore.isLoading" />
	</div>
</template>
