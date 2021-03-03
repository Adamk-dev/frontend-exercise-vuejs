<template>
  <b-card>
    <span class="state">{{ cardData.CITY }} - {{ cardData.STABBR }}</span>
    <h3>{{ cardData.INSTNM }}</h3>
    <span class="website">{{ cardData.INSTURL }}</span>
  </b-card>
</template>

<style scoped>
.card {
  background: #ffffff;
  box-shadow: 0px 2px 7px rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  padding: 8px;
  margin: 8px;
}
.state {
  font-family: Inter;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 150%;
  color: rgba(0, 0, 0, 0.6);
}
.website {
  font-family: Arial;
  font-style: normal;
  font-weight: normal;
  font-size: 15px;
  line-height: 150%;
  color: #e16259;
}
h3 {
  font-family: Arial;
  font-style: normal;
  font-weight: bold;
  font-size: 22px;
  line-height: 135%;
  color: #000000;
}
</style>

<script>
export default {
  name: "card",
  props: {
    cardData: {
      type: Object,
      default: null,
    },
  },
};
</script>
