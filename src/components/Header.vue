<template>
  <div>
    <b-nav tabs align="center">
      <b-nav-item disabled>
        <b>Quiz App</b>
      </b-nav-item>
      <b-nav-item disabled>Question: {{ questionIndex }}/10</b-nav-item>
      <b-nav-item disabled>Score: {{ score }}/100</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    questionIndex: Number,
    score: Number
  }
};
</script>
