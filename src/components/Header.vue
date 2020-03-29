<template>
<div>
  <b-nav tabs fill>
    <b-nav-item disabled>QuizzApp</b-nav-item>
    <b-nav-item disabled>Pokušaji:{{ numCorrect }} / {{ numTotal }}</b-nav-item>
  </b-nav>
</div>
</template>

<script>
  export default{
    props: [
      'numCorrect',
      'numTotal'
    ]
  }
</script>
