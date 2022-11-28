<template>
  <div class="wrapper">
    <p v-if="error">Something went wrong...</p>
    <p v-if="loading">Loading...</p>
    <table v-else>
      <thead>
        <tr>
          <th>Nome</th>
          <th>Email</th>
          <th>Telefone</th>
          <th>Acções</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="client in result.clients" :key="client.id">
          <td>{{ client.name }}</td>
          <td>{{ client.email }}</td>
          <td>{{ client.phone }}</td>
          <td class="action" @click="deleteClient({ id: client.id })">
            <i class="uil uil-trash"></i>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { useQuery, useMutation } from "@vue/apollo-composable";
import { GET_CLIENTS } from "@/utils/queries/clientQueries";
import { DELETE_CLIENT } from "@/utils/mutation/clientMutation";

export default {
  name: "HomeView",
  setup() {
    const { result, loading, error } = useQuery(GET_CLIENTS);
    const { mutate: deleteClient } = useMutation(DELETE_CLIENT, {
      // refetchQueries: [{ query: GET_CLIENTS }],
      update(cache, { data: { deleteClient } }) {
        const { clients } = cache.readQuery({ query: GET_CLIENTS });
        cache.writeQuery({
          query: GET_CLIENTS,
          data: {
            clients: clients.filter((client) => client.id !== deleteClient.id),
          },
        });
      },
    });
    return {
      result,
      loading,
      error,
      deleteClient,
    };
  },
};
</script>
<style scoped>
.wrapper {
  width: 769px;
  margin: 0 auto;
  padding: 1rem;
  background: rgba(51, 51, 51, 0.123);
}

tr {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 0.2fr;
  text-align: left;
  border-bottom: 1px solid #fff;
}

tbody tr td {
  text-align: left;
}
table {
  width: 100%;
}

th,
tbody tr {
  padding: 0.75rem 1rem;
}

.action:hover {
  cursor: pointer;
}
</style>
