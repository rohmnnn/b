+++
title = "Membuat Komponen Input Currency (Uang) dengan Masking"
date = "2026-11-17"
+++

Biar gampang kalau suatu saat butuh input nominal di aplikasi Vue, saya biasanya pakai komponen kecil yang formatnya otomatis jadi rupiah. Tujuannya supaya user tinggal ketik angka, lalu inputnya berubah jadi format uang secara real-time tanpa ribet. Cocok banget kalau dipakai di form transaksi, tagihan, atau pembelian.

<!--more-->

Kalau pakai Shadcn Vue, biasanya inputnya sudah punya style yang bagus. Yang perlu kita tambahkan cuma logika mask/format uangnya. Dengan `vue-currency-input`, kita bisa bikin komponen yang tetap menyimpan value sebagai number, tapi tampilannya tetap rapi seperti `Rp 1.234.567,00`.

## 1) Install dependency

```bash
npm install vue-currency-input
```

## 2) Buat komponen input uang

Berikut versi yang sering saya pakai. Komponen ini bisa langsung dipakai di project Vue 3 + Shadcn Vue.

```vue
<script setup>
import { watch } from "vue";
import { useCurrencyInput } from "vue-currency-input";
import { cn } from "@/lib/utils";

defineOptions({ inheritAttrs: false });

const props = defineProps({
    modelValue: { type: Number, default: null },
    name: String,
    options: { type: Object, default: () => ({}) },
});

const { inputRef, numberValue, setValue } = useCurrencyInput({
    locale: "id-ID",
    currency: "IDR",
    currencyDisplay: "symbol",
    precision: 2,
    autoDecimalDigits: false,
    hideCurrencySymbolOnFocus: false,
    hideGroupingSeparatorOnFocus: false,
    hideNegligibleDecimalDigitsOnFocus: false,
    ...props.options,
});

watch(
    () => props.modelValue,
    (value) => setValue(value),
);
</script>

<template>
    <input
        ref="inputRef"
        type="text"
        inputmode="numeric"
        v-bind="$attrs"
        :class="
            cn(
                'flex h-9 w-full rounded-md border border-input bg-transparent px-3 py-1 text-base shadow-sm transition-colors placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50 md:text-sm',
                props.class,
            )
        "
    />
    <input v-if="name" type="hidden" :name="name" :value="numberValue ?? ''" />
</template>
```

### Penjelasan singkat

- `locale: "id-ID"` -> format sesuai standar Indonesia
- `currency: "IDR"` -> mata uang rupiah
- `currencyDisplay: "symbol"` -> tampilkan `Rp`
- `precision: 2` -> dua digit desimal
- `numberValue` -> nilai numerik yang bisa dipakai untuk submit form
- `watch(() => props.modelValue, ...)` -> biar value bisa ter-sync saat parent component update

## 3) Cara pakai di form

```vue
<script setup>
import { ref } from "vue";
import CurrencyInput from "@/components/ui/CurrencyInput.vue";

const harga = ref(1250000);
</script>

<template>
    <div class="space-y-2">
        <label for="harga">Harga</label>
        <CurrencyInput v-model="harga" name="harga" />
    </div>
</template>
```

Kalau user mengetik `1250000`, tampilan di input akan berubah menjadi `Rp 1.250.000` (tergantung konfigurasi option). Sementara nilai di `harga` tetap berupa angka, jadi gampang dipakai untuk API, validasi, atau kalkulasi.

## 4) Kalau butuh custom format

Kadang kita butuh format tanpa desimal, atau mengubah style separator. Bisa lewat prop `options`.

```vue
<CurrencyInput
    v-model="jumlah"
    :options="{
        precision: 0,
        hideGroupingSeparatorOnFocus: true,
    }"
/>
```

Contoh hasilnya bisa jadi `Rp 1.250.000` tanpa angka desimal.

## 5) Kesimpulan

Komponen seperti ini sangat membantu kalau aplikasi kita sering menerima input nominal. Dengan sedikit konfigurasi, kita bisa punya input uang yang rapi, user-friendly, dan tetap aman untuk diproses secara programmatic.

Kalau dibutuhkan, saya juga bisa bikin versi yang lebih lengkap: misalnya support `min`, `max`, `prefix/suffix`, atau langsung jadi `Input` wrapper yang lebih cocok untuk form library seperti `vee-validate` / `zod`.
