<template>
    <div class="columns is-centered">
        <div class="column is-three-quarters-desktop is-full-touch">
            <enso-form class="box form-box"
                @loaded="handleLoaded"
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
                        :button="{
                            class: 'is-dark',
                            icon: faUser,
                            label: 'Edit User',
                        }"
                        @click="editUser"
                        v-if="canAccess('administration.users.edit')
                            && userId"/>
                    <action tag="a"
                        :button="{
                            class: 'is-dark',
                            icon: faUser,
                            label: 'Create User',
                        }"
                        @click="createUser"
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
                                    @update="count.Addresses = addressesRef.count"
                                    ref="addressesRef"/>
                            </div>
                        </div>
                    </tab>
                </template>
            </accessories>
        </div>
    </div>
</template>

<script setup>
import { computed, inject, ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { faUser } from '@fortawesome/free-solid-svg-icons';
import { Tab } from '@enso-ui/tabs/bulma';
import { EnsoForm, FormField, Action } from '@enso-ui/forms/bulma';
import Accessories from '@enso-ui/accessories/bulma';
import { Addresses } from '@enso-ui/addresses/bulma';

defineOptions({ name: 'Edit' });

const canAccess = inject('canAccess');
const routerErrorHandler = inject('routerErrorHandler');

const route = useRoute();
const router = useRouter();

const addressesRef = ref(null);
const companies = ref([]);
const form = ref(null);

const userCreate = {
    class: 'is-dark',
    icon: 'user',
    label: 'Create User',
};

const userEdit = {
    class: 'is-dark',
    icon: 'user',
    label: 'Edit User',
};

const personId = computed(() => Number.parseInt(route.params.person, 10));
const params = computed(() => ({
    id: companies.value,
}));
const userId = computed(() => form.value?.param('userId'));

const handleLoaded = () => {
    companies.value = form.value.field('companies').value;
};

const editUser = () => router.push({
    name: 'administration.users.edit',
    params: { user: userId.value },
}).catch(routerErrorHandler);

const createUser = () => router.push({
    name: 'administration.users.create',
    params: route.params,
}).catch(routerErrorHandler);
</script>
