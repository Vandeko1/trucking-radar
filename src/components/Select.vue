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

    /************/

    export default {
        name: 'Select',
        data() {
            return {
                options: [{
                        label: 'Crafter',
                        code: '1'
                    },
                    {
                        label: 'Sprinter',
                        code: '2'
                    },
                    {
                        label: 'Master',
                        code: '3'
                    },
                    {
                        label: 'Cangoo',
                        code: '4'
                    },
                    {
                        label: 'Transporter',
                        code: '5'
                    }
                ],
                selected: null
            }
        }
    }
</script>