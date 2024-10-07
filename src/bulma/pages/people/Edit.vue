<template>
    <div class="columns is-centered">
        <div class="column is-three-quarters-desktop is-full-touch">
            <enso-form class="box form-box has-background-light raises-on-hover"
                @loaded="companies = $refs.form.field('companies').value"
                ref="form">
                <template #companies="props">
                    <form-field v-bind="props"
                        @update:model-value="companies = $event"/>
                </template>
                <template #company="props">
                    <form-field v-bind="props"
                        :params="params"/>
                </template>
                <template #actions-left>
                    <action tag="a"
                        :button="userEdit"
                        @click="$router.push({
                            name: 'administration.users.edit',
                            params: { user: $refs.form.param('userId') }
                        }).catch(routerErrorHandler)"
                        v-if="canAccess('administration.users.edit')
                            && $refs.form.param('userId')"/>
                    <action tag="a"
                        :button="userCreate"
                        @click="$router.push({
                            name: 'administration.users.create',
                            params: $route.params,
                        }).catch(routerErrorHandler)"
                        v-else-if="canAccess('administration.users.create')"/>
                </template>
            </enso-form>
            <accessories>
                <template #default="{ count }">
                    <tab keep-alive
                        id="Addresses">
                        <div class="columns is-centered">
                            <div class="column is-two-thirds">
                                <addresses controls
                                    type="person"
                                    :id="personId"
                                    @update="count.Addresses = $refs.addresses.count"
                                    ref="addresses"/>
                            </div>
                        </div>
                    </tab>
                </template>
            </accessories>
        </div>
    </div>
</template>

<script>
import { Tab } from '@enso-ui/tabs/bulma';
import { EnsoForm, FormField, Action } from '@enso-ui/forms/bulma';
import Accessories from '@enso-ui/accessories/bulma';
import { Addresses } from '@enso-ui/addresses/bulma';

export default {
    name: 'Edit',

    components: {
        Accessories,
        Addresses,
        EnsoForm,
        Action,
        FormField,
        Tab,
    },

    inject: ['canAccess', 'i18n', 'routerErrorHandler'],

    data: () => ({
        companies: [],
        userCreate: {
            class: 'is-primary',
            icon: 'user',
            label: 'Create User',
        },
        userEdit: {
            class: 'is-warning',
            icon: 'user',
            label: 'Edit User',
        },
    }),

    computed: {
        personId() {
            return Number.parseInt(this.$route.params.person, 10);
        },
        params() {
            return {
                id: this.companies,
            };
        },
    },
};
</script>
