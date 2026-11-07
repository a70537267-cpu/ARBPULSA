<script setup lang="ts">
import { formatCurrency } from '@/lib/utils';
import { PPOBProductDataItem } from '@/types/cms/ppob';
import { Check, ClockIcon } from 'lucide-vue-next';

defineProps<{
    products: PPOBProductDataItem[];
    selectedProduct: number | null;
}>();

const emit = defineEmits<{
    'update:selectedProduct': [value: number];
}>();
</script>

<template>
    <section class="rounded-2xl border border-border/50 bg-card p-4 shadow-sm sm:p-6">
        <h2
            class="mb-4 flex items-center gap-2 text-lg font-bold text-foreground"
        >
            <span
                class="flex h-7 w-7 items-center justify-center rounded-full bg-primary text-sm font-bold text-primary-foreground"
                >2</span
            >
            Pilih Produk
        </h2>

        <div
            v-if="products && products.length > 0"
            class="grid grid-cols-2 gap-3 sm:grid-cols-3 lg:grid-cols-4"
        >
            <button
                v-for="product in products"
                :key="product.id"
                class="relative flex h-full flex-col overflow-hidden rounded-xl border-2 text-left transition-all duration-200 hover:-translate-y-0.5 hover:shadow-md"
                :class="
                    selectedProduct === product.id
                        ? 'border-primary bg-primary/10 shadow-md ring-1 ring-primary/20'
                        : 'border-border/60 bg-background/40 hover:border-primary/50'
                "
                @click="emit('update:selectedProduct', product.id)"
            >
                <div class="flex items-start justify-between gap-2 px-4 pt-4 text-sm font-semibold text-foreground">
                    {{ product.name }}
                    <Check v-if="selectedProduct === product.id" class="h-4 w-4 shrink-0 text-primary" />
                </div>
                <div
                    class="mt-2 flex flex-1 flex-col justify-between text-sm font-bold text-primary"
                >
                    <div class="flex items-center gap-2 pl-4 md:gap-3">
                        <div class="flex aspect-square w-6 items-center md:w-8">
                            <img
                                v-if="product.image"
                                :src="product.image"
                                :alt="product.name"
                                class="h-8 object-cover"
                                loading="lazy"
                            />
                        </div>
                        <div>
                            <span
                                class="flex items-center text-[14px] font-semibold text-primary md:text-[16px]"
                                id="headlessui-description-:r1u:"
                            >
                                {{ formatCurrency(product.sell_price) }}</span
                            >
                        </div>
                    </div>
                    <div
                        class="mt-5 flex w-full flex-col gap-2 border-t border-border/40 bg-muted/40 p-3 text-xs font-semibold text-foreground sm:p-4"
                    >
                        <div
                            class="flex items-center gap-2"
                            v-if="product.delay"
                        >
                            <ClockIcon
                                class="h-4 w-4 flex-shrink-0 text-muted-foreground"
                            />
                            <span>Delay</span>
                        </div>
                        <span
                            v-html="product.description"
                            class="text-muted-foreground"
                        ></span>
                    </div>
                </div>
            </button>
        </div>
        <div v-else class="py-8 text-center text-sm text-muted-foreground">
            Tidak ada produk tersedia
        </div>
    </section>
</template>
