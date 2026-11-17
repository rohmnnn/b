+++
title = "Reusable Komponen Select with Search Shadcn-vue"
date = "2025-11-21"
+++
Banyak project butuh select yang bisa dicari, bukan cuma dropdown biasa. Kalau datanya banyak, user pasti frustrasi kalau harus scroll satu-satu. Solusi paling praktis adalah bikin komponen reusable select dengan search, lalu style-nya tetap konsisten dengan Shadcn Vue.

<!--more-->

Kalau project kamu menggunakan Shadcn Vue, komponen `Combobox` dari reka-ui biasanya paling mudah dipakai. Kita bisa bikin wrapper reusable yang simple: menerima `options`, placeholder, dan `name`, lalu output value yang tinggal dipakai di form.

## 1) Install dependency yang dibutuhkan

Biasanya kalau project sudah punya Shadcn Vue, yang perlu ditambahkan tinggal package yang relevan. Pastikan juga komponen `combobox` sudah dibuat di project kamu.

```bash
npx shadcn-vue add combobox
```

## 2) Reusable component

```vue
<script setup>
import { computed } from "vue";
import { CheckIcon, ChevronsUpDownIcon } from "@lucide/vue";
import { Button } from "@/components/ui/button";
import {
    Combobox,
    ComboboxAnchor,
    ComboboxEmpty,
    ComboboxGroup,
    ComboboxInput,
    ComboboxItem,
    ComboboxItemIndicator,
    ComboboxList,
    ComboboxTrigger,
} from "@/components/ui/combobox";

const props = defineProps({
    name: String,
    options: { type: Array, default: () => [] },
    placeholder: { type: String, default: "Select..." },
    searchPlaceholder: { type: String, default: "Search..." },
});

const model = defineModel({ default: null });

const selected = computed({
    get: () => props.options.find((o) => o.value === model.value),
    set: (opt) => {
        model.value = opt?.value ?? null;
    },
});
</script>

<template>
    <div>
        <Combobox v-model="selected" by="value">
            <ComboboxAnchor as-child>
                <ComboboxTrigger as-child>
                    <Button
                        type="button"
                        variant="outline"
                        class="w-full justify-between font-normal"
                        :class="
                            selected?.label
                                ? ''
                                : 'text-muted-foreground hover:text-muted-foreground'
                        "
                    >
                        {{ selected?.label ?? placeholder }}
                        <ChevronsUpDownIcon class="opacity-50" />
                    </Button>
                </ComboboxTrigger>
            </ComboboxAnchor>

            <ComboboxList class="w-(--reka-combobox-trigger-width)">
                <ComboboxInput :placeholder="searchPlaceholder" />
                <ComboboxEmpty>Not found.</ComboboxEmpty>
                <ComboboxGroup>
                    <ComboboxItem
                        v-for="opt in options"
                        :key="opt.value"
                        :value="opt"
                    >
                        {{ opt.label }}
                        <ComboboxItemIndicator>
                            <CheckIcon />
                        </ComboboxItemIndicator>
                    </ComboboxItem>
                </ComboboxGroup>
            </ComboboxList>
        </Combobox>

        <input v-if="name" type="hidden" :name="name" :value="model ?? ''" />
    </div>
</template>
```

## 3) Format data options

Pastikan options-nya punya bentuk seperti ini:

```js
const roles = [
    { value: "admin", label: "Admin" },
    { value: "user", label: "User" },
    { value: "editor", label: "Editor" },
];
```

Lalu pakai di form seperti ini:

```vue
<script setup>
import { ref } from "vue";
import SelectWithSearch from "@/components/SelectWithSearch.vue";

const selectedRole = ref(null);
const roles = [
    { value: "admin", label: "Admin" },
    { value: "user", label: "User" },
    { value: "editor", label: "Editor" },
];
</script>

<template>
    <SelectWithSearch
        v-model="selectedRole"
        name="role"
        :options="roles"
        placeholder="Pilih role"
        search-placeholder="Cari role..."
    />
</template>
```

## 4) Kenapa ini berguna?

- reusable: satu komponen bisa dipakai berulang
- search: user bisa cari item tanpa scroll panjang
- tampilannya konsisten: cocok dengan desain Shadcn
- value tetap mudah diproses: kita hanya menyimpan `value`, bukan object utuh

## 5) Catatan kecil

Jika kamu mau komponen ini lebih fleksibel, bisa tambahkan props seperti:

- `disabled`
- `required`
- `emptyText`
- `className`
- `filterFn`

Dengan begitu komponen bisa dipakai ke banyak skenario tanpa perlu bikin ulang dari nol.

Intinya: kalau data pilihannya banyak, select dengan search jauh lebih nyaman dibanding dropdown biasa. Ini yang sering saya pakai saat bikin admin panel atau form yang butuh data pilihannya lengkap.
