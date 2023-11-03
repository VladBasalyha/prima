<template>
	<MainLayout>
		<div id="ItemPage" class="mt-4 max-w-[1200px] mx-auto px-2">
			<div class="md:flex gap-4 mx-auto w-full">
				<div class="md:w-[40%]">
					<img class="rounded-lg object-fit" :src="currentImage" />

					<div v-if="images[0] !== ''" class="flex items-center justify-center mt-2">
						<div v-for="image in images">
							<img
								@click="currentImage = image"
								width="70"
								class="rounded-md object-fit border-[3px] cursor-pointer w-full"
								:class="currentImage === image ? 'border-[#CC5500]' : ''"
								@mouseover="currentImage = image"
								:src="image"
								alt="product-picture" />
						</div>
					</div>
				</div>

				<div class="md:w-[60%] bg-white p-3 rounded-lg">
					<div v-if="product && product.data">
						<p class="uppercase font-bold mb-2">{{ product.data.title }}</p>
						<p class="font-light text-[12px] mb-2">{{ product.data.description }}</p>
					</div>

					<div class="flex items-center pt-1.5">
						<span class="bg-[#808080] h-4 min-w-4 rounded-full p-0.5 mr-2">
							<Icon name="ic:outline-star-purple500" class="-mt-[17px]" size="12" />
						</span>
						<p class="text-[#0]">10% discount</p>
					</div>

					<div class="flex items-center pt-1.5">
						<Icon name="ic:outline-star-purple500" class="-mt-[17px]" size="12" color="#808080" />
						<Icon name="ic:outline-star-purple500" class="-mt-[17px]" size="12" color="#808080" />
						<Icon name="ic:outline-star-purple500" class="-mt-[17px]" size="12" color="#808080" />
						<Icon name="ic:outline-star-purple500" class="-mt-[17px]" size="12" color="#808080" />
						<Icon name="ic:outline-star-purple500" class="-mt-[17px]" size="12" color="#808080" />
						<span class="text-[13px] font-light ml-2">1000 reviews 1.000+ orders</span>
					</div>
					<div class="border-b"></div>
					<div class="flex items-center justify-start gap-2 my-2">
						<div class="text-xl font-bold">
							${{ priceComputed }}
							<span class="border font-semibold px-1.5 rounded-sm text-[#CC5500] text-[9px] bg-[#F5F5F5]">70% off </span>
						</div>

						<p class="text-xs font-semibold pt-1 text-[#009A66]">Free delivery</p>

						<button
							:disabled="isInCart"
							@click="addToCart()"
							class="px-4 border-2 border-gray-500 py-1 rounded-lg text-gray-700 text-sm font-semibold bg-gradient-to-r from-[#809090] to-[#808080]">
							<div v-if="isInCart">Added</div>
							<div v-else>Add to cart</div>
						</button>
					</div>
				</div>
			</div>
		</div>
	</MainLayout>
</template>

<script setup>
	import MainLayout from '../../layouts/MainLayout';
	import { useUserStore } from '~/stores/user';
	const userStore = useUserStore();

	const route = useRoute();

	let product = ref(null);
	let currentImage = ref(null);

	onBeforeMount(async () => {
		product.value = await useFetch(`/api/prisma/get-product-by-id/${route.params.id}`);
	});

	watchEffect(() => {
		if (product.value && product.value.data) {
			currentImage.value = product.value.data.url;
			images.value[0] = product.value.data.url;
			userStore.isLoading = false;
		}
	});

	const isInCart = computed(() => {
		let res = false;
		userStore.cart.forEach((prod) => {
			if (route.params.id == prod.id) {
				res = true;
			}
		});

		return res;
	});

	const priceComputed = computed(() => {
		if (product.value && product.value.data) {
			return product.value.data.price / 100;
		}
		return '0.00';
	});

	const images = ref([
		'',
		'https://picsum.photos/id/212/800/800',
		'https://picsum.photos/id/213/800/800',
		'https://picsum.photos/id/214/800/800',
		'https://picsum.photos/id/322/800/800',
	]);

	const addToCart = () => {
		userStore.cart.push(product.value.data);
	};
</script>
