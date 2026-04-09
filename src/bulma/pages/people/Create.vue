<template>
    <div class="columns is-centered">
        <div class="column is-three-quarters-desktop is-full-touch">
            <enso-form class="box form-box">
                <template #companies="props">
                    <form-field v-bind="props"
                        @update:model-value="companies = $event"/>
                </template>
                <template #company="props">
                    <form-field v-bind="props"
                        :params="params"/>
                </template>
            </enso-form>
        </div>
    </div>
</template>

<script>
import { EnsoForm, FormField } from '@enso-ui/forms/bulma';

export default {
    name: 'Create',

    components: { EnsoForm, FormField },

    data: () => ({
        companies: [],
    }),

    computed: {
        params() {
            return {
                id: this.companies,
            };
        },
    },
};
</script>
