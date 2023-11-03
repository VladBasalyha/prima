<template>
	<MainLayout>
		<div id="IndexPage" class="m-auto max-w-[1200px]" mx-auto px-2>
			<div class="grid p-2 place-items-center xl:grid-cols-6 lg:grid-cols-5 md:grid-cols-4 sm:grid-cols-3 grid-cols-2 gap-4">
				<div v-if="products" v-for="product in products.data" :key="product">
					<ProductComponent :product="product" />
				</div>
			</div>
		</div>
	</MainLayout>
</template>
<script setup>
	import MainLayout from '~/layouts/MainLayout.vue';
	import { useUserStore } from '~/stores/user';

	const userStore = useUserStore();

	let products = ref(null);

	onBeforeMount(async () => {
		products.value = await useFetch(`/api/prisma/get-all-products`);

		setTimeout(() => {
			userStore.isLoading = false;
		}, 1000);
	});
	// const products = [
	// 	{
	// 		id: 1,
	// 		title: "Test product",
	// 		description: "lorem lorem text lorem",
	// 		url: "https://picsum.photos/id/237/200/300",
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 2,
	// 		title: "Test product",
	// 		description: "lorem lorem text lorem",
	// 		url: "https://picsum.photos/id/237/200/300",
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 3,
	// 		title: "Test product",
	// 		description: "lorem lorem text lorem",
	// 		url: "https://picsum.photos/id/237/200/300",
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 4,
	// 		title: "Test product",
	// 		description: "lorem lorem text lorem",
	// 		url: "https://picsum.photos/id/237/200/300",
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 5,
	// 		title: "Test product",
	// 		description: "lorem lorem text lorem",
	// 		url: "https://picsum.photos/id/237/200/300",
	// 		price: 1488,
	// 	},
	// 	{
	// 		id: 6,
	// 		title: "Test product",
	// 		description: "lorem lorem text lorem",
	// 		url: "https://picsum.photos/id/237/200/300",
	// 		price: 1488,
	// 	},
	// ];
</script>
