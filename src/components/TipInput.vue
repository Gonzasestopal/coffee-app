<template>
    <div class="form-row">
        <div class="form-field">
            <label for="tip">Do you wanna add tip?</label>
            <input type="text" name="tip" :value="tip" @input="setChildData"/>
        </div>
    </div>
</template>

<script>

export default {
    name: 'TipInput',
    data: function() {
        return {
            tip: ''
        }
    },
    methods: {
        setChildData: function(e) {
            this.$emit('set-child-data', e.data)
            this.tip = e.data;
        }
    }
}

</script>