<script setup lang="ts">
import { Check } from 'lucide-vue-next';
import { formatCurrency } from '@/lib/utils';

interface ManualBank {
    id?: string;
    name?: string;
    account_number?: string;
    account_name?: string;
    img?: string;
}

interface PaymentMethod {
    id: string;
    name: string;
    fee: number;
    action: 'add' | 'multiply';
    img: string;
}

defineProps<{
    paymentType: 'manual' | 'automatic';
    selectedPayment: string | null;
    manualBank: ManualBank;
    paymentMethods: PaymentMethod[];
}>();

const emit = defineEmits<{
    'update:paymentType': [value: 'manual' | 'automatic'];
    'update:selectedPayment': [value: string | null];
}>();

const handlePaymentTypeChange = (type: 'manual' | 'automatic') => {
    emit('update:paymentType', type);
    emit('update:selectedPayment', null);
};
</script>

<template>
    <section class="rounded-2xl border border-border/50 bg-card p-4 shadow-sm sm:p-6">
        <h2
            class="mb-4 flex items-center gap-2 text-lg font-bold text-foreground"
        >
            <span
                class="flex h-7 w-7 items-center justify-center rounded-full bg-primary text-sm font-bold text-primary-foreground"
                >4</span
            >
            Pilih Metode Pembayaran
        </h2>

        <!-- Payment Type Selector -->
        <div class="mb-4 flex gap-2">
            <button
                class="flex-1 rounded-xl border-2 px-4 py-3 text-sm font-semibold transition-all"
                :class="
                    paymentType === 'automatic'
                        ? 'border-primary bg-primary/5 text-primary'
                        : 'border-border/50 text-muted-foreground hover:border-primary/50'
                "
                @click="handlePaymentTypeChange('automatic')"
            >
                Otomatis
            </button>
            <button
                class="flex-1 rounded-lg border-2 px-4 py-2 text-sm font-medium transition-all"
                :class="
                    paymentType === 'manual'
                        ? 'border-primary bg-primary/5 text-primary'
                        : 'border-border/50 text-muted-foreground hover:border-primary/50'
                "
                @click="handlePaymentTypeChange('manual')"
            >
                Manual (Transfer Bank / QRIS)
            </button>
        </div>

        <!-- Manual Payment - Bank Accounts -->
        <div v-if="paymentType === 'manual'" class="space-y-2">
            <button
                :key="manualBank.id"
                class="flex w-full items-center gap-3 rounded-xl border-2 p-3 transition-all sm:gap-4 sm:p-4"
                :class="
                    selectedPayment === manualBank.id
                        ? 'border-primary bg-primary/10 shadow-sm'
                        : 'border-border/60 hover:border-primary/50'
                "
                @click="emit('update:selectedPayment', manualBank.id as any)"
            >
                <img
                    :src="manualBank.img"
                    :alt="manualBank.name"
                    class="h-8 w-12 object-contain"
                    loading="lazy"
                />
                <div class="flex-1 text-left">
                    <div class="font-medium text-foreground">
                        {{ manualBank.name }}
                    </div>
                    <div class="text-xs text-muted-foreground">
                        {{
                            manualBank.account_number
                                ? manualBank.account_number + ' - '
                                : ''
                        }}
                        {{ manualBank.account_name }}
                    </div>
                </div>
                <Check v-if="selectedPayment === manualBank.id" class="h-5 w-5 shrink-0 text-primary" />
                <span class="text-xs font-semibold text-emerald-500 sm:text-sm">
                    Fee: Rp 0
                </span>
            </button>
        </div>

        <!-- Automatic Payment Methods -->
        <div v-else class="space-y-2">
            <button
                v-for="method in paymentMethods"
                :key="method.id"
                class="flex w-full items-center gap-4 rounded-lg border-2 p-4 transition-all"
                :class="
                    selectedPayment === method.id
                        ? 'border-primary bg-primary/5'
                        : 'border-border/50 hover:border-primary/50'
                "
                @click="emit('update:selectedPayment', method.id)"
            >
                <img
                    :src="method.img"
                    :alt="method.name"
                    class="h-8 w-12 object-contain"
                    loading="lazy"
                />
                <span class="flex-1 text-left font-medium text-foreground">
                    {{ method.name }}
                </span>
                <Check v-if="selectedPayment === method.id" class="h-5 w-5 shrink-0 text-primary" />
                <span class="text-xs text-muted-foreground sm:text-sm">
                    Fee:
                    {{
                        method.action === 'multiply'
                            ? `${(method.fee * 100).toFixed(1)}%`
                            : formatCurrency(method.fee)
                    }}
                </span>
            </button>
        </div>
    </section>
</template>
