<template>
	<MainLayout>
		<div id="ShoppingCart" class="mt-4 max-w-[1200px] mx-auto px-6">
			<div v-if="userStore.cart.length === 0" class="flex flex-col items-center justify-center h-[500px]">
				<div class="pt-20 mb-6">
					<img class="mx-auto" width="250" src="../public/images/empty-cart.png" alt="empty cart image" />
					<p class="font-extrabold text-center text-gray-700 text-xl">Oops, your shopping cart is empty, let's add something here!</p>

					<div v-if="!user" class="flex text-center">
						<NuxtLink to="/auth" class="bg-[#00394f] w-full text-white text-[22px] font-semibold p-2 py-2 rounded-full mt-8"> Sign In</NuxtLink>
					</div>
				</div>
			</div>

			<div v-else class="md:flex gap-4 justify-center mx-auto w-full">
				<div class="md:w-[65%]">
					<div class="bg-gray-200 rounded-lg p-4">
						<div class="text-center text-2xl font-bold mb-4">Shopping cart ({{ userStore.cart.length }})</div>
					</div>
					<div class="bg-[#ffeeff] rounded-lg p-4 mt-4">
						<div class="text-center text-red-300 font-bold">Welcome Deal applicable on 1 item</div>
					</div>
					<div id="Items" class="bg-gray-200 rounded-lg p-4 mt-4">
						<div v-for="product in userStore.cart">
							<CartItem :product="product" :selectedArray="selectedArray" @selectedRadio="selectedRadioFunc" />
						</div>
					</div>
					<div class="md:hidden block my-4"></div>
				</div>
				<div class="md:w-[35%]">
					<div id="Summary" class="bg-gray-200 rounded-lg p-4">
						<div class="text-2xl font-extrabold mb-2">Summary</div>
						<div class="flex items-center justify-between my-4">
							<div class="text-xl font-bold">Total</div>
							<div class="text-xl font-semibold">
								$ <span class="font-bold">{{ totalPriceComputed }}</span>
							</div>
						</div>
						<button
							class="flex uppercase items-center justify-center bg-[#fe3211] w-full text-white text-[19px] font-semibold p-1.5 rounded-full mt-4"
							@click="goToCheckout">
							Checkout
						</button>
					</div>

					<div class="bg-gray-200 rounded-lg p-4 mt-4" id="PaymentProtection">
						<div class="text-lg font-semibold mb-2">Payment methods</div>
						<div class="flex items-center justify-start gap-8 my-4">
							<div v-for="card in cards">
								<img height="80" width="60" class="h-12" :src="card" alt="card image" />
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</MainLayout>
</template>

<script setup>
	import MainLayout from '~/layouts/MainLayout.vue';
	import { useUserStore } from '~/stores/user';

	const user = useSupabaseUser();
	const userStore = useUserStore();
	const cards = ref(['images/visa.png', 'images/mastercard.png', 'images/paypal.png', 'images/applepay.png']);

	const totalPriceComputed = computed(() => {
		let price = 0;
		userStore.cart.forEach((prod) => {
			price += prod.price;
		});

		return price / 100;
	});

	let selectedArray = ref([]);
	const selectedRadioFunc = (e) => {
		if (!selectedArray.value.length) {
			selectedArray.value.push(e);
			return;
		}
		selectedArray.value.forEach((item) => {
			if (e.id != item.id) {
				selectedArray.value.push(e);
			} else {
				selectedArray.value.splice(index, 1);
			}
		});
	};

	const goToCheckout = () => {
		let ids = [];
		userStore.checkout = [];
		selectedArray.value.forEach((item) => ids.push(item.id));

		let res = userStore.cart.filter((item) => {
			return ids.indexOf(item.id) != -1;
		});
		res.forEach((item) => userStore.checkout.push(toRaw(item)));
		return navigateTo('/checkout');
	};
	onMounted(() => {
		setTimeout(() => (userStore.isLoading = false), 200);
	});

	// const products = [
	// 	{
	// 		id: 1,
	// 		title: 'Test product',
	// 		description: 'lorem lorem text lorem',
	// 		url: 'https://picsum.photos/id/237/200/300',
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 2,
	// 		title: 'Test product',
	// 		description: 'lorem lorem text lorem',
	// 		url: 'https://picsum.photos/id/237/200/300',
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 3,
	// 		title: 'Test product',
	// 		description: 'lorem lorem text lorem',
	// 		url: 'https://picsum.photos/id/237/200/300',
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 4,
	// 		title: 'Test product',
	// 		description: 'lorem lorem text lorem',
	// 		url: 'https://picsum.photos/id/237/200/300',
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 5,
	// 		title: 'Test product',
	// 		description: 'lorem lorem text lorem',
	// 		url: 'https://picsum.photos/id/237/200/300',
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 6,
	// 		title: 'Test product',
	// 		description: 'lorem lorem text lorem',
	// 		url: 'https://picsum.photos/id/237/200/300',
	// 		price: 1488,
	// 	},
	// ];
</script>
