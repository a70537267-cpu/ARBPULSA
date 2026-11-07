<script setup lang="ts">
import { index } from '@/actions/App/Http/Controllers/Main/HomeController';
import BrandCard from '@/components/BrandCard.vue';
import HeroBanner from '@/components/HeroBanner.vue';
import MainFooter from '@/components/MainFooter.vue';
import MainHeader from '@/components/MainHeader.vue';
import Maintenance from '@/pages/main/Maintenance.vue';
import { PaginationItem } from '@/types';
import { PPOBBrandDataItem, PPOBCategoryDataItem } from '@/types/cms/ppob';
import { SliderDataItem } from '@/types/cms/web';
import { Head, InfiniteScroll, Link, usePage } from '@inertiajs/vue3';

defineProps<{
    sliders: SliderDataItem[];
    brands: PaginationItem<PPOBBrandDataItem>;
    featured_brands: PPOBBrandDataItem[];
    categories: PPOBCategoryDataItem[];
}>();

const page = usePage();
const setting = page.props.setting;
const appUrl = page.props.app_url;
</script>

<template>
    <Head>
        <title>{{ setting?.title }}</title>
        <component :is="'script'" type="application/ld+json">
            {{
                JSON.stringify({
                    '@context': 'https://schema.org',
                    '@type': 'WebSite',
                    name: setting?.title,
                    url: index().url,
                    potentialAction: {
                        '@type': 'SearchAction',
                        target: {
                            '@type': 'EntryPoint',
                            urlTemplate: `${index().url}?search={search_term_string}`,
                        },
                        'query-input': 'required name=search_term_string',
                    },
                })
            }}
        </component>
    </Head>

    <div class="flex min-h-screen flex-col bg-background">
        <!-- Header -->
        <MainHeader />

        <template v-if="setting?.maintenance_status === 'active'">
            <Maintenance />
        </template>
        <!-- Main Content -->
        <main class="mx-auto w-full max-w-7xl flex-1 px-4 py-8" v-else>
            <!-- Welcome / Brand Intro -->
            <section class="relative mb-6 overflow-hidden rounded-2xl border border-primary/20 bg-gradient-to-br from-blue-950 via-slate-900 to-violet-950 px-5 py-6 shadow-lg sm:px-8 sm:py-8">
                <div class="pointer-events-none absolute -right-12 -top-16 h-48 w-48 rounded-full bg-blue-500/20 blur-3xl"></div>
                <div class="pointer-events-none absolute -bottom-20 left-1/3 h-40 w-40 rounded-full bg-violet-500/20 blur-3xl"></div>
                <div class="relative max-w-2xl">
                    <span class="mb-3 inline-flex rounded-full border border-white/20 bg-white/10 px-3 py-1 text-xs font-semibold tracking-wide text-blue-100">
                        TOP UP GAME & PRODUK DIGITAL
                    </span>
                    <h1 class="text-2xl font-extrabold tracking-tight text-white sm:text-3xl">
                        Top Up Mudah, Cepat, dan Aman
                    </h1>
                    <p class="mt-2 max-w-xl text-sm leading-6 text-slate-200 sm:text-base">
                        Pilih game atau produk digital favoritmu, masukkan ID tujuan, lalu lanjutkan pesanan dengan praktis.
                    </p>
                </div>
            </section>

            <!-- Hero Banner -->
            <HeroBanner
                class="mb-10 overflow-hidden rounded-2xl shadow-md"
                :slides="sliders"
                :autoplay="true"
                :interval="50000"
            />

            <!-- Categories Section -->
            <section class="mb-10">
                <div class="mb-4 flex items-end justify-between gap-3">
                    <div>
                        <p class="text-xs font-semibold uppercase tracking-wider text-primary">Jelajahi</p>
                        <h2 class="text-xl font-bold text-foreground">Kategori</h2>
                    </div>
                </div>
                <div class="flex gap-3 overflow-x-auto pb-2">
                    <Link
                        v-for="category in categories"
                        :href="index().url + '?category=' + category.slug"
                        :key="category.id"
                        preserve-scroll
                    >
                        <button
                            class="flex min-w-[140px] flex-col items-center gap-2 rounded-xl border border-border/50 bg-card p-4 shadow-sm transition-all hover:-translate-y-1 hover:border-primary/50 hover:shadow-lg"
                        >
                            <img
                                v-if="category.image"
                                :src="category.image"
                                alt="category.name"
                                class="h-12 w-12 object-contain"
                                loading="lazy"
                            />
                            <span class="text-3xl" v-else> 🎮 </span>
                            <span class="text-sm font-semibold text-foreground">
                                {{ category.name }}
                            </span>
                            <span class="text-xs text-muted-foreground">
                                {{ category.active_brands_count }} produk
                            </span>
                        </button>
                    </Link>
                </div>
            </section>

            <!-- Featured Brands Section -->
            <section class="mb-10" v-if="featured_brands.length > 0">
                <div class="mb-4">
                    <p class="text-xs font-semibold uppercase tracking-wider text-primary">Pilihan terbaik</p>
                    <h2 class="text-xl font-bold text-foreground">
                        Produk Unggulan
                    </h2>
                </div>
                <div
                    class="grid grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6"
                >
                    <BrandCard
                        v-for="brand in featured_brands"
                        :key="brand.id"
                        :brand="brand"
                    />
                </div>
            </section>

            <!-- Brands Section -->
            <section>
                <div class="mb-6">
                    <p class="text-xs font-semibold uppercase tracking-wider text-primary">Temukan favoritmu</p>
                    <h2 class="text-xl font-bold text-foreground">
                        Produk Populer
                    </h2>
                </div>

                <InfiniteScroll data="brands">
                    <div
                        class="grid grid-cols-2 gap-4 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6"
                    >
                        <BrandCard
                            v-for="brand in brands?.data"
                            :key="brand.id"
                            :brand="brand"
                        />
                    </div>
                </InfiniteScroll>
            </section>
        </main>

        <!-- Footer -->
        <MainFooter />
    </div>
</template>
