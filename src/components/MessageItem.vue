<template>
  <div>
      <b-row v-if="isAgent">
        <b-col>
          <b-card border-variant="info">
            <b-card-text>{{ message }}</b-card-text>
          </b-card>
        </b-col>
        <b-col></b-col>
        <b-col></b-col>
      </b-row>
      <b-row v-else>
        <b-col></b-col>
        <b-col></b-col>
        <b-col>
          <b-card border-variant="info">
            <b-card-text>{{ message }}</b-card-text>
          </b-card>
        </b-col>
      </b-row>
  </div>
</template>

<script>
export default {
    props: ['isAgent', 'message'],
};
</script>

<style>
</style>