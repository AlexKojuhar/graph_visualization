<template>
    <div class="form--container" >
        <b-tabs content-class="mt-3">
            <b-tab title="Create Node" active>
            <b-input></b-input>            
        </b-tab>
        <b-tab title="Create Link">
            <b-form-select :options="options"></b-form-select>
        </b-tab>
        </b-tabs>
    </div>
</template>
<script>
    import axios from "axios";
    export default {
        name: 'Form',
        data() {
            return {
                options: []
            }
        },
        methods:{
             all() {
                axios.get('http://127.0.0.1:8000/graph/nodes/').then(response => {
                    // eslint-disable-next-line no-console
                    console.log(response.data)
                })
            },
        },
        created () {
           this.all()
        }
    }
</script>
<style>

</style>