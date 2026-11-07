<script setup lang="ts">
import { show } from '@/actions/App/Http/Controllers/Main/BrandController';
import { PPOBBrandDataItem } from '@/types/cms/ppob';
import { Link } from '@inertiajs/vue3';
import { ShoppingBag } from 'lucide-vue-next';

const props = defineProps<{
    brand: PPOBBrandDataItem;
}>();
</script>

<template>
    <div
        class="group relative overflow-hidden rounded-2xl border border-border/50 bg-card shadow-sm transition-all duration-300 hover:-translate-y-1 hover:border-primary/50 hover:shadow-xl"
    >
        <Link
            :href="
                show({
                    brand: brand.slug,
                }).url
            "
        >
            <!-- Brand Image -->
            <div class="relative aspect-[4/3] overflow-hidden bg-gradient-to-br from-muted to-background">
                <img
                    v-if="brand.image"
                    :src="brand.image"
                    :alt="brand.name"
                    class="h-full w-full object-contain p-3 transition-transform duration-300 group-hover:scale-105"
                    loading="lazy"
                />
                <div
                    v-else
                    class="flex h-full w-full items-center justify-center text-muted-foreground"
                >
                    <ShoppingBag class="h-15 w-15 opacity-20" />
                </div>

                <!-- Overlay on hover -->
                <div
                    class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent opacity-0 transition-opacity duration-300 group-hover:opacity-100"
                ></div>
            </div>

            <!-- Brand Info -->
            <div class="flex min-h-[76px] items-center justify-between gap-2 p-3 sm:p-4">
                <!-- Name -->
                <h3
                    class="line-clamp-2 text-sm font-semibold leading-5 text-foreground"
                >
                    {{ brand.name }}
                </h3>
                <span class="shrink-0 rounded-full bg-primary/10 px-2.5 py-1 text-[10px] font-bold text-primary">PILIH</span>
            </div>
        </Link>
    </div>
</template>
