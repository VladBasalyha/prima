<template>
	<MainLayout>
		<div id="OrdersPage" class="mt-4 max-w-[1200px] mx-auto px-2 min-h-[50px]">
			<div class="bg-gray-200 w-full p-8 min-h-[120px]">
				<div class="flex items-center text-xl">
					<Icon name="streamline:shipping-transfer-truck-time-truck-shipping-delivery-time-waiting-delay" size="35" />
					<span class="pl-4 font-extrabold">Orders</span>
				</div>

				<div v-if="orders && orders.data" v-for="order in orders.data" class="text-md pl-[50px]">
					<div class="border-b-2 border-b-gray-400 py-2">
						<p>Stripe ID: {{ order.stripeId }}</p>
						<div class="pt-2"></div>
						<div v-for="item in order.orderItem">
							<NuxtLink :to="`/item/${item.productId}`" class="flex items-center gap-3 p-1 hover:underline hover:text-gray-700">
								<img width="70" height="70" :src="item.product.url" alt="item image" />
								{{ item.product.title }}
							</NuxtLink>
						</div>
						<div class="pt-2 pb-5">
							Delivery addres: {{ order.name }}, {{ order.address }}, {{ order.zipcode }}, {{ order.city }}, {{ order.country }}
						</div>
					</div>
				</div>

				<div v-else class="flex items-center justify-center">OOOPS, you don't have any orders yet</div>
			</div>
		</div>
	</MainLayout>
</template>

<script setup>
	import MainLayout from '~/layouts/MainLayout.vue';

	import { useUserStore } from '~/stores/user';
	const userStore = useUserStore();
	const user = useSupabaseUser();

	let orders = ref(null);

	onBeforeMount(async () => {
		orders.value = await useFetch(`/api/prisma/get-all-orders-by-user/${user.value.id}`);
	});

	onMounted(() => {
		if (!user.value) {
			return navigateTo('/auth');
		}

		setTimeout(() => (userStore.isLoading = false), 200);
	});
</script>
