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
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="client in result.clients" :key="client.id">
          <td>{{ client.name }}</td>
          <td>{{ client.email }}</td>
          <td>{{ client.phone }}</td>
          <td class="action">
            <i class="uil uil-home"></i>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import gql from "graphql-tag";
import { useQuery } from "@vue/apollo-composable";

const GET_CLIENTS = gql`
  query getClients {
    clients {
      id
      name
      email
      phone
    }
  }
`;

export default {
  name: "HomeView",
  setup() {
    const { result, loading, error } = useQuery(GET_CLIENTS);
    return {
      result,
      loading,
      error,
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
  display: flex;
  justify-content: space-between;
  text-align: left;
  border-bottom: 1px solid #fff;
}
table {
  width: 100%;
}

th,
tbody tr {
  padding: 0.75rem 1rem;
}
</style>
