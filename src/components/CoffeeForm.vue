<template>
    <form @submit.prevent="submitForm">
        <LocaleSelect></LocaleSelect>
        <div class="form-row">
            <div class="form-field">
                <label for="drink">{{ $t("drink") }}</label>
                <select v-model="formData.drink" name="drink" required>
                    <option disabled selected value="">{{ $t("select") }}</option>
                    <option :key="drink" :value="drink.toLowerCase()" v-for="drink in drinks">{{ drink }}</option>
                    <option value="filtrado">{{ $t("filter") }}</option>
                </select>
            </div>
        </div>
        <div v-show="formData.drink" class="form-row">
            <div class="form-field">
                <label for="origin">{{ $t("origin") }}</label>
                <select v-model="formData.origin" name="origin">
                    <option disabled selected value="">{{ $t("select") }}</option>
                    <option :key="origin" :value="origin.toLowerCase()" v-for="origin in origins">{{ origin }}</option>
                </select>
            </div>
        </div>
        <div v-show="toggleMilk" class="form-row">
            <div class="form-field">
                <label for="milk">{{ $t("milkType") }}</label>
                <select v-model="formData.milk" name="milk">
                    <option disabled selected value="">{{ $t("select") }}</option>
                    <option :key="milkType" :value="milkType.toLowerCase()" v-for="milkType in milkTypes">{{ $t(milkType.toLowerCase()) }}</option>
                </select>
            </div>
        </div>
        <div v-show="toggleSize" class="form-row">
            <div class="form-field">
                <label for="size">{{ $t("size")}}</label>
                <select v-model="formData.size" name="size">
                    <option disabled selected value="">{{ $t("select") }}</option>
                    <option :key="size" :value="size.toLowerCase()" v-for="size in sizes">{{ size }}</option>

                </select>
            </div>
        </div>
        <div class="form-row">
            <div class="form-field">
                <textarea v-model.trim="formData.comment" name="comment" v-bind:placeholder="$t('request')"></textarea>
            </div>
        </div>

        <TipInput v-on:set-child-data="updateParent"></TipInput>

        <p for="">{{ $t('total') }} ${{ calculateTotal }}</p>

        <input v-bind:style="{ 'background-color': submitBackgroundColor, 'color': submitColor }" type="submit"
            v-bind:disabled="!formIsValid" :value="$t('submit')">
    </form>
</template>

<script>
import TipInput from './TipInput.vue'
import LocaleSelect from './LocaleSelect.vue';
import prices from '../domain/prices';
import origins from '../domain/origins';
import drinks from '../domain/drinks';
import milkTypes from '../domain/milkTypes';
import sizes from '../domain/sizes';

export default {
    name: 'CoffeeForm',
    components: {
        TipInput,
        LocaleSelect,
    },
    data: function () {
        return {
            formData: {
                origin: "guatemala",
                drink: '',
                comment: '',
                milk: '',
                sabor: '',
                tip: '',
                size: '',
            },
            formDataCopy: '',
            origins: origins,
            drinks: drinks,
            milkTypes: milkTypes,
            sizes: sizes,
        }
    },
    mounted: function() {
        this.formDataCopy = { ...this.formData };
    },
    computed: {
        size: {
            get: function () {
                if (this.formData.origin == 'guatemala') {
                    return 'venti'
                }
                return this.formData.size;
            },
            set: function(value) {
                this.size = value;
            }
        },
        formIsValid: function () {
            if (this.formData.drink == 'espresso') {
                return this.formData.origin;
            } else if (this.formData.drink == 'filtrado') {
                return this.formData.origin && this.size;
            } else if (['latte','cappuccino'].includes(this.formData.drink)) {
                return this.formData.origin && this.size && this.formData.milk;
            }
            return false
            
        },
        submitBackgroundColor: function () {
            if (!this.formIsValid) {
                return 'gray'
            }
            return '#ffffff'
        },
        submitColor: function () {
            if (!this.formIsValid) {
                return 'white'
            }
            return 'black'
        },
        toggleMilk: function () {
            return ["cappuccino", "latte"].includes(this.formData.drink)
        },
        toggleSize: function () {
            return ["cappuccino", "latte", "filtrado"].includes(this.formData.drink)
        },
        calculateTotal: function () {
            return prices[this.formData.drink]  + Number(this.formData.tip) || 0
        },
    },
    methods: {
        submitForm: function () {
            this.resetFormData()

        },
        updateParent: function (newTip) {
            this.formData.tip = newTip
        },
        resetFormData() {
            this.formData = { ...this.formDataCopy };
        },
    }
}
</script>


<i18n>
{
    "en": {
        "total": "Your total is",
        "select": "Select one option",
        "request": "Please add any additional requests here",
        "size": "Select your coffee size",
        "drink": "Select your favourite beverage",
        "milkType": "Select your milk type",
        "origin": "Select your coffee origin",
        "submit": "Submit order",
        "filter": "Drip coffee",
        "almonds": "Alomnds",
        "whole": "Whole",
        "skimmed": "Skimmed"
    },
    "es": {
        "total": "Tu total es de",
        "select": "Selecciona una opcion",
        "request": "Por favor escribe aqui si tienes alguna peticion adicional",
        "size": "Selecciona el tamaño de tu bebida",
        "drink": "Selecciona tu bebida favorita",
        "milkType": "Selecciona tu tipo de bebida",
        "origin": "Selecciona el origen de tu cafe",
        "submit": "Realizar pedido",
        "filter": "Filtrado",
        "almonds": "Almendras",
        "whole": "Entera",
        "skimmed": "Deslactosada"
    }
}
</i18n>
