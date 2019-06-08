<template>
    <v-data-table
        :headers="headers"
        :items="mails"
        :pagination.sync="pagination"
        :loading="loading"
        :search="search"
        class="elevation-1"
        >
        <template v-slot:items="props">
        <td>
            <div class="ma-3">
            <h3>{{ props.item.data.contact.name }}</h3>
            <p class="subheading">{{ props.item.data.contact.email }}</p>
            <p>Date de réception : {{ props.item.created_at | formatDate }}</p>
            </div>
        </td>
        <td>
            <div class="ma-3">
            {{ props.item.data.contact.message }}
            </div>
        </td>
        </template>
    </v-data-table>
</template>

<script>
export default {
    props: {
        mails: {
            type: Array,
            Default: [],
        }
    },
    data() {
        return {
            headers: [
                { text: 'Informations', sortable: false },
                { text: 'Message', value: 'data.contact.message', sortable: false },
            ],
            pagination: {'sortBy': 'create_at', 'descending': true},
        }
    },
    watch: {
        mails(){
            this.$emit('mails')
        }
    },
}
</script>