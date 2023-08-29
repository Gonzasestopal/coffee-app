<template>
    <form @submit.prevent="submitForm">
        <div class="form-row">
            <div class="form-field">
                <label for="drink">Bebida</label>
                <select v-model="formData.drink" name="drink" required>
                    <option disabled selected value="">Selecciona una opcion</option>
                    <option value="latte">Latte</option>
                    <option value="espresso">Espresso</option>
                    <option value="filtrado">Filtrado</option>
                    <option value="cappuccino">Capuccino</option>
                </select>
            </div>
        </div>
        <div v-show="formData.drink" class="form-row">
            <div class="form-field">
                <label for="origin">Origen</label>
                <select v-model="formData.origin" name="origin">
                    <option disabled selected value="">Selecciona una opcion</option>
                    <option value="etiopia">Etiopia</option>
                    <option value="guatemala">Guatemala</option>
                    <option value="chiapas">Chiapas</option>
                </select>
            </div>
        </div>
        <div v-show="toggleMilk" class="form-row">
            <div class="form-field">
                <label for="milk">Tipo de leche</label>
                <select v-model="formData.milk" name="milk">
                    <option disabled selected value="">Selecciona una opcion</option>
                    <option value="whole">Whole</option>
                    <option value="skimmed">Skimmed</option>
                    <option value="almonds">Almonds</option>
                </select>
            </div>
        </div>
        <div v-show="toggleSize" class="form-row">
            <div class="form-field">
                <label for="size">Tamaño</label>
                <select v-model="size" name="size">
                    <option disabled selected value="">Selecciona una opcion</option>
                    <option value="venti">Venti</option>
                    <option value="grande">Grande</option>
                    <option value="alto">Alto</option>
                </select>
            </div>
        </div>
        <div class="form-row">
            <div class="form-field">
                <textarea v-model.trim="formData.comment" name="comment" placeholder="Alguna peticion adicional"></textarea>
            </div>
        </div>

        <TipInput v-on:set-child-data="updateParent"></TipInput>

        <p for="">Tu total es de $ {{ calculateTotal }}</p>

        <input v-bind:style="{ 'background-color': submitBackgroundColor, 'color': submitColor }" type="submit"
            v-bind:disabled="!formIsValid" value="Order">
    </form>
</template>

<style scoped>
label {
    color: red
}
</style>

<script>
import TipInput from './TipInput.vue'

const prices = {
    latte: 3,
    cappuccino: 4,
    filtrado: 2,
    espresso: 2,
}


export default {
    name: 'CoffeeForm',
    components: {
        TipInput,
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
            },
            formDataCopy: '',
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
                return ''
            },
            set: function(value) {
                return value
            }
        },
        formIsValid: function () {
            if (this.formData.drink == 'espresso') {
                return this.formData.origin;
            } else if (this.formData.drink == 'filtrado') {
                return this.formData.origin && this.size;
            } else if (['latte','capuccino'].includes(this.formData.drink)) {
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
            console.log('omg', this.formData.drink)
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
