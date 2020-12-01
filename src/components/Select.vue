<template>
    <div>
        <p><b>Оберіть авто:</b></p>
        <v-select v-model="selected" :options="options" @input="$emit('select', selected)"></v-select>
    </div>
</template>
<script>
    import Vue from 'vue'
    import vSelect from 'vue-select'
    Vue.component('v-select', vSelect)
    export default {
        name: 'Select',
        props: ['vehicles'],
        data() {
            return {
                options: this.vehicles,
                selected: null
            }
        }
    }
</script>