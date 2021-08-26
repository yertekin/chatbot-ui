<template>
  <div id="messageItem">
      <b-row v-if="agent">
        <b-col cols="8">
          <b-card class="card-agent">
            <b-card-text>{{ message }}</b-card-text>
          </b-card>
        </b-col>
        <b-col cols="4"></b-col>
      </b-row>
      <b-row v-else>
        <b-col cols="4"></b-col>
        <b-col >
          <b-card class="card-user">
            <b-card-text>{{ message }}</b-card-text>
          </b-card>
        </b-col>
      </b-row>
  </div>
</template>

<script>
export default {
    props: ['agent', 'message'],
};
</script>

<style>
.card-agent {
  background-color: #124b8f !important;
  color: white;
  border-radius: 1rem !important;
}
.card-user{
  border-radius: 1rem !important;
}
</style>
