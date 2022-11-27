<template>
  <p v-if="error">Something went wrong...</p>
  <p v-if="loading">Loading...</p>
  <p v-else v-for="(client, index) in result.clients" :key="index">
    {{ client.name }}
  </p>
  <div></div>
</template>

<script>
import gql from "graphql-tag";
import { useQuery } from "@vue/apollo-composable";

const GET_CLIENTS = gql`
  query getClients {
    clients {
      name
      email
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
