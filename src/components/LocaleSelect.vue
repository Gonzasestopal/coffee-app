<template>
    <div class="form-row">
        <div class="form-field">
            <label for="locale">{{ $t("locale") }}</label>
            <select v-model="$root.$i18n.locale" name="locale" required>
                <option disabled selected value="">{{ $t("select") }}</option>
                <option value="en">{{ $t("en") }}</option>
                <option value="es">{{ $t("es") }}</option>
            </select>
        </div>
    </div>
</template>

<script>
export default {
    name: "LocaleSelect"
}
</script>

<i18n>
{
    "en": {
        "en": "English",
        "es": "Spanish",
        "locale": "Select your language of choice",
        "select": "Select one option"
        
    },
    "es": {
        "en": "Ingles",
        "es": "Español",
        "locale": "Selecciona el idioma que prefieras",
        "select": "Selecciona una opcion"
    }
}
</i18n>
