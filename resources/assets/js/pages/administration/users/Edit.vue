<template>

    <div class="columns is-centered">
        <div class="column is-three-quarters">
            <vue-form :data="form"
                class="box animated fadeIn"
                v-if="initialised">
                <template slot="owner_id" slot-scope="{ field, errors }">
                    <vue-select name="owner_id"
                        v-model="field.value"
                        :key-map="field.meta.keyMap"
                        :has-error="errors.has(field.name)"
                        @input="pivotParams.owners.id=$event;errors.clear(field.name)"
                        :source="field.meta.source">
                    </vue-select>
                </template>
                <template slot="role_id" slot-scope="{ field, errors }">
                    <vue-select name="role_id"
                        :pivot-params="pivotParams"
                        v-model="field.value"
                        :key-map="field.meta.keyMap"
                        :has-error="errors.has(field.name)"
                        @input="errors.clear(field.name);"
                        :source="field.meta.source">
                    </vue-select>
                </template>
            </vue-form>
        </div>
    </div>

</template>

<script>

import VueForm from '../../../components/enso/vueforms/VueForm.vue';
import VueSelect from '../../../components/enso/select/VueSelect.vue';

export default {
    components: { VueForm, VueSelect },

    data() {
        return {
            initialised: false,
            form: {},
            pivotParams: { owners: { id: null } },
        };
    },

    created() {
        axios.get(route(this.$route.name, this.$route.params.id, false)).then(({ data }) => {
            this.form = data.form;
            this.pivotParams.owners.id = this.getOwnerId();
            this.initialised = true;
        }).catch(error => this.handleError(error));
    },

    methods: {
        getOwnerId() {
            const attribute = this.form.fields.find(({ name }) => name === 'owner_id');

            return attribute.value;
        },
    },
};

</script>
